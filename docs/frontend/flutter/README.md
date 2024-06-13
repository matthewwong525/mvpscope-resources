# Flutter

## Run flutter local web server

1. [Install flutter](https://docs.flutter.dev/get-started/install)
2. In your terminal, run the following commands:

```bash
git clone https://github.com/devtodollars/flutter-supabase-production-template.git YOUR_APP_NAME
cd YOUR_APP_NAME
```

3. Run the local web server.

```bash
cd flutter
flutter run -d chrome --dart-define-from-file=env.json
```

:::info
The Supabase backend used is the same one as [https://app.devtodollars.com](https://app.devtodollars.com) . See how to [setup your own backend](../../supabase/README.md).
:::

## Flutter Project Structure

* `/flutter/android,ios,linux,macos,web,windows` -> Directories containing native code
* `/flutter/lib`
  * `/flutter/lib/screens` -> A container for components that is typically passed into the router
  * `/flutter/lib/components` -> Components that are contained within the screens
  * `/flutter/lib/services` -> Utility or [state management](misc/state-management.md) classes are stored here
  * `/flutter/lib/models` -> Class or interface definitions that are passed around
* `/flutter/env.json` -> Environment variables for production
* `/flutter/env.local.json` -> Environment variables for [local development](../../supabase/supabase-local-development.md)
* `/flutter/bumpversion.sh` -> Script used for [upgrading the app version](release.md)

:::warning
Do NOT put sensitive keys into `env.json` or `env.local.json`
:::

## Helpful Links

* [https://docs.flutter.dev/get-started/install](https://docs.flutter.dev/get-started/install)
