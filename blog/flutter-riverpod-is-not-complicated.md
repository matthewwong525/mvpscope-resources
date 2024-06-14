---
title: Flutter Riverpod is Not Complicated - Getting Started with Riverpod
description: There seems to be a lot of confusion with Riverpod and the way it is used. Admittedly the documentation is lacking. And for someone getting started, there are many decisions to be made.
image: /assets/riverpod-banner.png
date: 2024-03-29
---

There seems to be a lot of confusion with Riverpod and the way it is used. Admittedly the [documentation is lacking](https://www.reddit.com/r/FlutterDev/comments/11omsdq/comment/jbu56su/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button). And for someone getting started, there are many decisions to be made like:

- Should I use code-generation?
- How many providers should I create?
- What should be contained in each provider?

Because of this adaptability, it can become very confusing for someone just getting started. I'm creating this blog post to lay some ground rules that I set for myself when using riverpod. If you're getting started with riverpod, following these rules will be a good starting point.

<!-- truncate -->

But before reading on these rules, I highly recommend you checkout these guides in this order:

1. [Flutter Riverpod 2.0: The Ultimate Guide](https://codewithandrea.com/articles/flutter-state-management-riverpod/)
2. [How to Auto-Generate your Providers with Flutter Riverpod Generator](https://codewithandrea.com/articles/flutter-riverpod-generator/)
3. [How to use Notifier and AsyncNotifier with the new Flutter Riverpod Generator](https://codewithandrea.com/articles/flutter-riverpod-async-notifier/)

## Basics

Because I know some of you are lazy as hell, I'll summarize what I think is important in the below bullet points:

- Riverpod is like a global variable storage and each provider is it's own global variable.
- Only special widgets `ConsumerWidget` and `ConsumerStatefulWidget` have access to these providers.
- You can access the providers using `ref.read` and `ref.watch`
  - `ref.watch` is used in the Widget's `build` method rebuilds the widget the state changes
  - `ref.read` is used outside of the Widget's `build` method
- There are many different types of providers to choose from and the riverpod generator makes it so you don't need to choose which one to use.
- There are different modifiers you can apply to the provider when accessing it.
  - By default you get the `AsyncValue` with no modifiers
  - `.notifier` can be used to access the functions within the provider
  - `.future` can be used to get the latest value of the state asynchronously
- An `AsyncValue` is returned when accessing the provider with no modifiers
  - `.when` is typically used in the Widget `build` method
  - `.value` is to get the current value

## Common Pitfalls of Riverpod

### Not Using Code Generation

I personally hate code generation. It adds an extra generated file and it abstracts logic that might be important to understand.

Because of reasons above, I decided to give riverpod a try without code generation. After a couple of times, of choosing the wrong provider, encountering bugs because of incorrect parameters, I decided that code generation was the way forward.

After I gave it a shot, everything became simple. It saved me hours of hair pulling trying to configure the correct parameters for each provider. Even the riverpod documentation [highly recommends code generation](https://riverpod.dev/docs/concepts/about_code_generation#should-i-use-code-generation).

### Grouping Providers based on Technology

When first working with riverpod, I thought the best approach would be to group global variables by the technology. For example, I had a library for my database, I put all my database related functions in the single provider and called it a day. My thinking was that this was just a global variable storage

But by doing this, I lost a lot of the capabilities riverpod provided out of the box. I had to:

- Refresh the UI with `ref.watch` based on specific criteria
- I had to manage the states myself which added unnecessary complexity
- Handle the initialization of states and loading states manually

If you want to see how NOT to use riverpod, I encourage you to checkout [how I did it incorrectly with Fleeting Notes](https://github.com/fleetingnotes/fleeting-notes-flutter/blob/main/lib/services/providers.dart).

### Not Using Streams

Streams are so so powerful. If you have a database that supports streaming I highly recommend you use streams to _streamline_ your setup. There's no more need to handle updates, inserts, or deletes, they are automatically done so with your backend being the source of truth.

## Examples

Below are two very common use cases for production applications. One is with authentication and the second is with routing.

### Authentication

Below is a simplified version for learning purposes. Checkout the [full code here](https://github.com/devtodollars/flutter-production-template/blob/main/flutter/lib/services/auth_notifier.dart).

```dart
@Riverpod(keepAlive: true)
class Auth extends _$Auth {
  // We use a stream controller to control when the stream is updated and what object is in the stream.
  final StreamController<AppUser?> authStateController =
      StreamController.broadcast();

  Auth();

  @override
  Stream<AppUser?> build() {
    // listen to auth state change
    final streamSub = client.auth.onAuthStateChange.listen((authState) async {
      refreshUser(authState);
    });

    // dispose the listeners
    ref.onDispose(() {
      streamSub.cancel();
      authStateController.close();
    });

    // return the stream
    return authStateController.stream;
  }

  supa.SupabaseClient get client => supa.Supabase.instance.client;

  Future<AppUser?> refreshUser(supa.AuthState state) async {
    final session = state.session;
    if (session == null) {
	  // set the auth state to null
      authStateController.add(null);
      return null;
    }

    // Make an additional query to get subscription data
    final metadata = await client
        .from("stripe")
        .select()
        .eq("user_id", session.user.id)
        .maybeSingle();

    // Put together custom user object
    final user = AppUser(
      session: session,
      authEvent: state.event,
      activeProducts: List<String>.from(metadata?["active_products"] ?? []),
      stripeCustomerId: metadata?["stripe_customer_id"],
    );

    // update the stream
    authStateController.add(user);
    return user;
  }
}
```

## Routing

Below is a simplified version for learning purposes. Checkout the [full code here](https://github.com/devtodollars/flutter-production-template/blob/main/flutter/lib/services/router_notifier.dart).

```dart
// This is crucial for making sure that the same navigator is used
// when rebuilding the GoRouter and not throwing away the whole widget tree.
final navigatorKey = GlobalKey<NavigatorState>();
Uri? initUrl = Uri.base; // needed to set intiial url state

@riverpod
GoRouter router(RouterRef ref) {
  // we watch the authState to update the route when auth changes
  final authState = ref.watch(authProvider);
  return GoRouter(
    initialLocation: initUrl?.path, // DO NOT REMOVE
    navigatorKey: navigatorKey,
    redirect: (context, state) async {
      // we redirect the user based on different criteria of auth
      return authState.when(
        data: (user) {
          // build initial path
          String? path = initUrl?.path;
          final queryString = initUrl?.query.trim() ?? "";
          if (queryString.isNotEmpty && path != null) {
            path += "?$queryString";
          }
          // If user is not authenticated, direct to login screen
          if (user == null && path != '/login') {
            return '/login';
          }
          // If user is authenticated and trying to access login or loading, direct to home
          if (user != null && (path == '/login' || path == '/loading')) {
            return "/";
          }
          // After handling initial redirection, clear initUrl to prevent repeated redirections
          initUrl = null;
          return path;
        },
        error: (_, __) => "/loading",
        loading: () => "/loading",
      );
    },
    routes: <RouteBase>[
      GoRoute(
        name: 'loading',
        path: '/loading',
        builder: (context, state) {
          return const Center(child: CircularProgressIndicator());
        },
      ),
      GoRoute(
        name: 'login',
        path: '/login',
        builder: (context, state) {
          return const AuthScreen();
        },
      ),
      GoRoute(
        name: 'home',
        path: '/',
        builder: (context, state) {
          return const HomeScreen(title: "DevToDollars");
        },
      ),
    ],
  );
}
```

## What's Next?

If you liked what you saw, I encourage you to checkout my [Flutter production boilerplate](https://github.com/devtodollars/flutter-production-template). This boilerplate doesn't only get you up and running in under 30 minutes, but also is a good resource if you're building your own app.
