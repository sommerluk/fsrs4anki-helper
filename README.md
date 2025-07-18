# FSRS Helper

[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)

FSRS Helper is an Anki add-on that supports [FSRS](https://github.com/open-spaced-repetition/fsrs4anki) algorithm. It has eight main features:

- **Reschedule** cards based on their entire review histories.
- **Postpone** a selected number of due cards.
- **Advance** a selected number of undue cards.
- **Balance** the load during rescheduling (based on fuzz).
- Less Anki on **Easy Days** (such as weekends) during rescheduling (based on load balance).
- **Disperse** Siblings (cards with the same note) to avoid interference & reminder.
- **Flatten** future due cards to a selected number of reviews per day.
- **Steps Stats** quantify your short-term memory performance and recommend learning steps.

# Requirements

- For Anki version >= 23.10
  - Enable built-in FSRS (follow this [tutorial](https://github.com/open-spaced-repetition/fsrs4anki/blob/main/docs/tutorial.md))
  - Remove FSRS4Anki custom scheduling code if you are already using it
- For Anki version in 2.1.55 - 2.1.66 (no longer maintained)
  - Enable V3 Scheduler
  - FSRS4Anki version >= 3.0.0

# Installation

The FSRS Helper add-on is purely an added bonus and is not recommended for extensive use.

Installation link: https://ankiweb.net/shared/info/759844606

# Usage

## Overview

| Feature name      | How does it work?                                            | When should I use it?                                        |
| ----------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Reschedule        | Calculates stability, difficulty, and the optimum interval from the entire review history for each card using FSRS parameters. Then, it changes the due dates of cards. | When you update the parameters or desired retention of FSRS. However, this is not necessary anymore, as Anki has a built-in feature "Reschedule cards on change". |
| Advance           | Decreases the intervals of undue cards based on current and requested R, and interval length to minimize damage to long-term learning. | When you want to review your material ahead of time, for example, before a test. |
| Postpone          | Increases the intervals of cards that are due today based on current and requested R, and interval length in a way that minimizes damage to long-term learning. | When you are dealing with a large number of reviews after taking a break from Anki or after rescheduling. |
| Load Balancing    | After the optimal interval is calculated, it is adjusted by a random amount to make the distribution of reviews over time more uniform. | Always. This feature makes your workload (reviews per day) more consistent. |
| Easy Days         | After the optimal interval is calculated, it is slightly adjusted to change the due date. | If you want to spend less time on Anki on some days of the week, for example, Sundays. |
| Disperse Siblings | Siblings are cards generated from the same note. Their intervals are adjusted to spread them further apart from each other. | Always. This feature alleviates the interference; disabling it will only decrease the efficiency of spaced repetition. |
| Flatten           |  If the number of future due cards exceeds the limit, the cards are postponed. | When you want to spread your backlog to the future. |

## Reschedule

Rescheduling can calculate the memory states and intervals based on each card's review history and the parameters from the Scheduler code. These parameters can be personalized with the FSRS Optimizer.

**Note**: For cards that have been reviewed multiple times using Anki's default algorithm, rescheduling may give different intervals than the Scheduler because the Scheduler can't access the full review history when running. In this case, the intervals given by rescheduling will be more accurate. But after rescheduling once, there will be no difference between the two.

![image](https://github.com/user-attachments/assets/1ca82fe5-9750-4586-97ec-55ef8c9f39df)

## Advance/Postpone

These two functions are very similar, so I'll talk about them together. You can set the number of cards to advance/postpone, and the Helper add-on will sort your cards and perform the advance/postpone in such a way that the deviation from the original review schedule is minimal while meeting the number of cards you set.

![image](https://github.com/user-attachments/assets/56ff6f97-c111-43f6-be1c-43415dbb5e0b)

![image](https://github.com/open-spaced-repetition/fsrs4anki-helper/assets/32575846/f9838010-cb00-44ce-aefc-10300f2a586e)

## Load Balance

FSRS Helper automatically applies load balance when you reschedule all cards via the add-on.

Here's a comparison, the first graph is rescheduling via the Anki's built-in method, and the second graph is via the add-on:

![image](https://github.com/open-spaced-repetition/fsrs4anki-helper/assets/32575846/1f31491c-7ee6-4eed-ab4a-7bc0dba5dff8)

![image](https://github.com/open-spaced-repetition/fsrs4anki-helper/assets/32575846/1c4f430d-824b-4145-801e-68fc0329fbbd)

## Easy Days

You can select any day or days from Monday through Sunday to take off. Once enabled, the Helper add-on will try to avoid these days when rescheduling. On "Reduced" days, you will only have ~50% the usual amount of reviews. On "Minimum" days, there will be ~0% reviews.

**Update**: after Anki 24.11, the easy days feature is built-in. So you don't need to configure it in the add-on settings.

Note: Easy Days only works for cards in the "review" stage. Due to technical limitations, FSRS doesn't modify the interval and due date of cards in the "(re)learning" stage. And it also doesn't reschedule cards whose interval is less than 3 days.

Fuzz example:
- Review less than 3 days: will not choose another day.
- Review in 3 days: Choose between days 2 and 4.
- Review in 7 days: Choose between days 5 and 9.
- Review in 90 days: Choose between days 86 and 94.
- Cards you forgot, and return within 2 days it does not choose another day.

Exceptions:

If the fuzz range is too narrow or does not exist (review less than 3 days) to satisfy the selected easy days, the day of the week may be selected for review of the card.

**Effect**:

![image](https://github.com/user-attachments/assets/79c5eda3-b4c8-4694-95c4-f88a6cd84118)


## Disperse Siblings

In Anki, some templates will generate multiple cards related in content from the same note, such as reversed cards (Front->Back, Back->Front) and cloze cards (when you make multiple clozes on the same note). If the review dates of these cards are too close, they may interfere with or remind you of each other. Dispersing siblings can spread the review dates of these cards out as much as possible.

![image](https://github.com/user-attachments/assets/3e358cb7-e135-4d8a-9dc5-3056bb881d8c)

## Flatten

You can set the maximum number of reviews per day for future days. The Helper add-on will find out the days that exceed the limit and postpone the extra cards to the future.

## Advanced Search (<=2.1.66)

In the card browser, you can right-click on the header and click on Difficulty, Stability, Retention to display the current memory states of cards.

![image](https://github.com/open-spaced-repetition/fsrs4anki-helper/assets/32575846/7fb2b357-19d0-45fb-9cc0-f925258a6280)

The Helper also adds [search syntax](https://docs.ankiweb.net/searching.html) for these three properties, here are some examples:

- s<10: Cards with memory stability less than 10 days
- d=5: Cards with difficulty equal to 5
- r<0.6: Cards with memory retrievability (recall probability) less than 60%

In Anki 23.10+, you can use the built-in search syntax to search for them as [card properties](https://docs.ankiweb.net/searching.html#card-properties).

## Advanced Statistics

Hold down the Shift key and click "Stats" to enter the old version of Anki's statistics interface.

### FSRS Stats

![](https://github.com/user-attachments/assets/88b7b713-c3a0-4eb3-acb3-83585f4119fd)

The FSRS Stats are based on all cards in your deck or collection (whichever is selected) that you have ever reviewed. They remain unaffected by the 1 month/year settings.

Interpretation:

- **Daily Load** is an estimate of the average review count required per day. It is influenced by the [Burden](https://supermemo.guru/wiki/Burden) statistic in SuperMemo. 
- The [three component of the memory model](https://supermemo.guru/wiki/Three_component_model_of_memory) used by FSRS:
	- Average predicted **retention** reflects the percentage of cards that you would recall correctly if you were tested today.
	- Average (memory) **stability** reflects how fast you forget (forgetting rate). The greater the stability, the slower the forgetting rate.

### Steps Stats

![](https://github.com/user-attachments/assets/3d1a9865-654d-4074-8eb5-2f905a4fdf69)

The Steps Stats are based on the cards from the selected deck or collection that were first reviewed in the last 1 month/year or deck life and have at least two reviews.

These stats are helpful for fine-tuning your (re)learning steps to achieve your desired retention in the short-term reviews. 

How your learning steps affect the intervals:
- If your first rating for a new card is Again, the interval is the 1st learning step.
- If your first rating is Again and the second one is Good, the interval is the 2nd learning step.
- If your first rating is Good, the interval is the 2nd learning step.
- If your first rating is Hard, the interval is (1st learning step + 2nd learning step) / 2.

So the 1st recommended learning step is based on the stability of your cards where you press Again during the first review.

For the 2nd recommended learning step, the scenario is more complex. It is based on the minimum of the stability of three kinds of cards:
- S(Again Then Good): Your ratings are Again and then Good for a new card.
- S(Good): Your first rating is Good.
- S(Hard): Your first rating is Hard.
  - Because a Hard step is (1st learning step + 2nd learning step) / 2, the 2nd learning step is 2 * hard step - 1st learning step. And the Hard step is based on S(Hard).

## Other features

- **Auto reschedule cards reviewed on other devices after sync:** This option is useful if you do some (or all) of your reviews on platforms that don't support FSRS such as AnkiDroid or AnkiWeb. If this option is enabled, the reviews synced from the other devices will be automatically rescheduled according to the FSRS algorithm. If you are relying on this feature, it is recommended to sync the reviews daily for the best results.
- **Auto reschedule the card you just reviewed (<=2.1.66):** If you enable this option, every card that you review will be rescheduled. Enabling this option is not essential for using FSRS. It is mainly intended for gradually transitioning your old cards to FSRS when starting to use FSRS. The other option for transitioning old cards to FSRS is to reschedule all cards, but this tends to induce a huge backlog for many people. Other advantages of enabling the "Auto reschedule the card you just reviewed" option include:
    - Load balance and easy days are applied when rescheduling.
    - It allows you to use learning or relearning steps longer than or equal to 1 day without breaking the scheduling. However, for best results, it is not recommended to use such steps even with this option enabled because FSRS can determine the next intervals more accurately.

    However, this option also has some disadvantages, which include:
    - The intervals displayed above the answer buttons may be inconsistent with the real interval after rescheduling, though the real ones will be more optimal.
    - It might have a small effect on the responsiveness of Anki and introduce lags because it needs more calculations for each review and causes constant queue rebuilding.
- **Auto disperse siblings when review:** It automatically disperses siblings after each review. But it could cause constant queue rebuilding, which slows down Anki and breaks Display Order settings.
- **Reschedule all cards:** This option is used to reschedule all the cards in the decks in which FSRS is enabled. It should only be used after you have installed FSRS for the first time and/or updated your parameters.
- **Reschedule cards reviewed in the last 7 days:** This option can be used to reschedule the cards that were reviewed in the last few days. The number of days can be adjusted in the add-on config.
- **Update scheduler:** This option can be used to check for updates to the FSRS scheduler and update the scheduler code if an update is available. While updating the scheduler code, this option preserves your existing configuration.

# Mechanism

Please see this wiki page: [FSRS Helper WIKI](https://github.com/open-spaced-repetition/fsrs4anki-helper/wiki)

# Support My Work

I'm Jarrett Ye, a diehard fan of spaced repetition. I used Anki during my high school life and attended my dream college. Now, I'm a researcher and developer in the field of spaced repetition. FSRS is my homage to Anki, expressing my deep gratitude for its invaluable support.

If you like FSRS Helper, [please Rate this!👍](https://ankiweb.net/shared/review/759844606) Your support will allow more people to enjoy this fantastic tool and improve their learning experience!

*A kind request*: Writing, supporting, and maintaining FSRS takes considerable time and effort. If this tool has become a valuable asset in your studies, please consider showing your appreciation by clicking the button below to make a contribution on Ko-fi. Every bit of support is greatly appreciated and will go a long way in helping me maintain and improve FSRS over time!

<a href='https://ko-fi.com/X8X6LQZM4' target='_blank'><img height='32' width="127"
    style='border:0px;height:32px;' src='https://storage.ko-fi.com/cdn/kofi1.png?v=6' border='0'
    alt='Buy Me a Coffee at ko-fi.com' /></a>

Or support me in GitHub: [Sponsor @L-M-Sherlock on GitHub Sponsors](https://github.com/sponsors/L-M-Sherlock)

# Acknowledgements

I referred to the following projects while developing FSRS Helper:

- [load balancer](https://github.com/jakeprobst/anki-loadbalancer)
- [Free Weekend](https://github.com/cjdduarte/Free_Weekend)
- [Delay siblings](https://github.com/oakkitten/anki-delay-siblings)
- [True Retention by Card Maturity Simplified](https://ankiweb.net/shared/info/1779060522)

Thanks to all the contributors for their valuable contributions!
