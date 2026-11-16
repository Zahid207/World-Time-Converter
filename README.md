# README

# World Time Converter

A single-page tool that converts a date and time from any country and state to any other country and state. You pick a place on each side, enter a time, and get the same moment on the other clock.

**Live demo:** [World Time Converter](https://worldtimeconverter.vercel.app/)

## Why this exists

Most time converters ask for an exact time zone name or only list a few big cities. That breaks down for countries with several time zones, such as the United States, Canada and Australia, where "country" alone doesn't tell you the time. This tool lets you choose the country first and then the state or region, so you don't need to know the time zone name.

## Features

- Country and state/region selection on both the "From" and "To" sides
- State and province lists for the United States, Canada, Australia, Brazil, Russia, Mexico and Indonesia, where states can differ in time
- State or division lists for countries that share one time zone, including India, Bangladesh, Pakistan, Nepal, China, Japan, Germany, France, Italy, Spain and the United Kingdom
- More than 50 single-time-zone countries available directly
- An "Other" option that lists every IANA time zone your browser supports
- 12-hour or 24-hour clock, set separately for the input and the result
- Automatic daylight saving time handling
- UTC offset for both places and the hour difference between them
- Swap button and "Use current time" button
- A 24-block bar showing how far through the day it is at the destination
- Responsive layout: a landscape layout for desktop screens, and a stacked layout for tablets and phones

## Usage

1. Choose the country and state or region you are converting **from**.
2. Enter the date and time. Use "Use current time" to fill in the current time for that place.
3. Choose the country and state or region you are converting **to**.
4. Read the result. Use the swap button to reverse the direction.

## Run locally

There is no build step and no dependencies. Download the repository and open the file in a browser.

```bash
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>
open index.html        # macOS
# start index.html     # Windows
# xdg-open index.html  # Linux
```

## How it works

- Time zones come from the browser's built-in `Intl` API, which uses the IANA time zone database. No external requests are made.
- The offset of a zone at a given moment is found with `Intl.DateTimeFormat().formatToParts()`. The entered local time is converted to UTC with that offset, checked a second time to handle daylight saving changes, and then formatted in the destination zone.
- States and regions are labels that map to a time zone. In countries with one time zone, every state maps to the same zone.

## Project structure

```
.
├── index.html   # markup, styles and script in one file
└── README.md
```

## Customize the data

The place data is at the top of the script in `index.html`.

- **Add a single-time-zone country:** add `Country:Area/City` to the `single` string. For example, `Kenya:Africa/Nairobi`.
- **Add states to a country:** add an entry to the `ST` object with a comma-separated list. All states use the country's time zone.
- **Give one state a different zone:** write it as `State:Area/City` inside that list, as with `Canary Islands:Atlantic/Canary` under Spain.
- **Add a country with several time zones:** add an entry to the `multi` object, with each region mapped to its IANA zone.

IANA zone names are listed on [Wikipedia's list of tz database time zones](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones).

## Limitations

- Not every country has a state list yet. Countries without one show a single entry, and you can use the "Other" option for any zone.
- Countries with several time zones are mapped by main region. A state that is split across zones uses its most common zone.
- Time zone rules come from your browser. An out-of-date browser may have out-of-date daylight saving rules.
- The `datetime` handling covers normal dates. Times that don't exist or occur twice during a daylight saving change are resolved to one valid result.
