---
title: How I reached 50k users with Flutter
description: Hey, I'm Matt, once the CTO of a startup backed by YC. Today, I'm here to unravel the steps I took to reach 50,000 users and how you can replicate this success!
image: /assets/50k-flutter-banner.png
date: 2024-03-16
---

Hey, I'm Matt, once the CTO of a startup backed by YC. Today, I'm here to unravel the steps I took to reach 50,000 users and how you can replicate this success!

<!-- truncate -->

### 1. Identifying a problem

Everything began with a problem that needed solving. In my scenario, this was the tedious task med school students faced creating flashcards from their study material, dedicating precious hours each week to this alone.

### 2. Understanding my customers & competitors

Understanding your customers means REALLY getting to know them. I took a trip to California, visiting Stanford and UCLA, to talk directly with over 50 med school students. Coupled with deep dives into online forums and Reddit, I gathered invaluable insights:

- **Who are my users?** They are med students in the U.S., buried under 70+ hours of study each week, all while preparing for the USMLE exam while doing their clinical placements. Improving their study efficiency by even 20% could save them invaluable hours.
- **What is their current workflow?** Although their study methods vary, they universally rely on question banks as a testing tool. The challenge here lies in ensuring they don’t repeat the same mistakes, which they tackle through various means like manual flashcards, Excel spreadsheets, or using complex software to track their progress.
- **What does my product offer?** I developed an AI-powered flashcard generator designed to seamlessly integrate into their study routine, allowing for the automatic creation of flashcards from their question bank questions with just a click. This innovation significantly streamlines their study process, eliminating the need for manual flashcard creation.

### 3. Validating my idea

Before diving into product development, validating the idea was crucial. I crafted a Reddit post [here](https://www.reddit.com/r/step1/comments/19ayx2p/study_uworld_more_efficiently/) that attracted over 90 upvotes which led to numerous direct messages. This was the first sign of potential success, but the true test is whether or not they would pay for it.

### 4. Building my landing page

The decision to opt for a no-code webpage builder over Flutter for the landing page was influenced by three key reasons:

1. **Flutter's shortcomings for landing pages**, notably [slower load times](https://github.com/flutter/flutter/issues/76009) and [subpar SEO capabilities](https://github.com/flutter/flutter/issues/46789).
2. **Empowering my non-technical co-founder** to make edits, significantly enhancing our agility in making iterations.
3. **The fast speed of deployment** offered by no-code solutions.

### 5. Web App Development

With the landing page set up and the concept validated, I moved on to developing the web application. Opting to [focus solely on a web app](flutter-vs-react-building-a-startup-on-the-web.md) was driven by two considerations:

- **Speed**: Immediate deployment, avoiding the drawn-out approval processes associated with app stores, and the complications of mobile-specific setups.
- **Accessibility**: The universal ease of accessing a web link, simplifies user feedback and eliminates the need for app store downloads.

### 6. Iterating Based on Customer Feedback

Following the launch, I reached out to the med students I initially spoke with, seeking their feedback. This step was critical, as their input directly influenced our prioritization of features, including the consideration of mobile support for future development. To date, we haven't added a mobile app, but it remains a goal on our roadmap.

### 7. Continuing to Grow

For us, leveraging TikTok through partnerships with micro-influencers was a game-changer, generating viral content that led to spikes in users.

Throughout this journey, our strategy was to experiment, identify what worked, cut what didn’t, and double down on successful tactics.

### What Now?

Thanks for taking the time to read the post. I'm currently on a mission to help developers build their startups and I hope that my experience is helpful for you! If you want to build a startup with Flutter, checkout my [open-source flutter boilerplate](https://github.com/devtodollars/flutter-production-template).
