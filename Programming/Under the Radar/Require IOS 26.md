#UnderTheRadar
### Highlights

- **The Annual Dilemma:** The hosts discuss the yearly question for iOS developers: whether to drop support for the previous iOS version (in this case, iOS 18) and require the new one (iOS 26) right away.
    
- **Developer vs. Business Needs:** The conversation centers on the conflict between the developer's desire for a simpler, <mark style="background: #BBFABBA6;">more productive workflow by only supporting the latest iOS</mark>, and the business need to avoid alienating existing users and <mark style="background: #FF5582A6;">cutting off potential new customers on older systems.</mark>
    
- **Data-Driven Decisions:** David Smith emphasizes using data to inform this decision, citing his own app, Widgetsmith. He notes that even a year after a new iOS release, <mark style="background: #ADCCFFA6;">a small but significant percentage of new downloads still come from users on the older OS</mark>.
    
- **A "Crazy" but Effective Solution:** David proposes a unique strategy: creating a parallel, <mark style="background: #D2B3FFA6;">duplicated version of the app's view hierarchy</mark>. This allows developers to build a "pure" iOS 26 experience without breaking compatibility for users on iOS 18, as the app essentially runs two different versions of the UI based on the user's operating system.
    
- **Sponsorship:** The episode is sponsored by Sentry, an error monitoring and tracing platform, which is presented as a tool to help developers spend less time on debugging and more on building features.