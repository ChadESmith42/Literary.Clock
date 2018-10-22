# Literary.Clock
Web clock that displays literary quotes for user's system time.

## Literary Clock
There are several walk-throughs and tutorials on creating a project like this. I was drawn into the concept by [Jaap Meijers](https://www.instructables.com/id/Literary-Clock-Made-From-E-reader/), who used an old Kindle to display the quotes.

## Quote Data
I used the database of literarly clock quotes hosted by [The Guardian](https://www.theguardian.com/books/booksblog/2013/jul/03/guardian-literary-clock) for their physical literary clock. The dataset can be found [here](https://www.theguardian.com/books/table/2011/apr/21/literary-clock). I created a JSON object to hold the data. This is an example of the JSON formatting for the quote data:
```javascript
    {
        "time": "11:52:00 PM",
        "descriptive": "eight minutes to midnight",
        "quote": "It was eight minutes to midnight. Just nice time, I said to myself. Indoors, everything was quiet and in darkness. Splendid. I went to the bar and fetched a tumbler, a siphon of soda and a bottle of Glen Grant, took a weak drink and a pill, and settled down in the public dining-room to wait the remaining two minutes.",
        "source": "The Green Man",
        "author": "Kingsley Amis"
    },
```
## Web Clock
I wanted to make a pure browser version that (should) work in modern browsers and mobile devices. The literary.js file contains all of the quotes in a JSON object and the clock code. The program creates a `Date` object to capture the current time from the user's system. That variable is then compared to each object in the JSON to find a matching time, using an `hh:mm a` format. The matching `object.quote`, `object.author`, and `object.source` is then passed to the Index.html page for display. The page automatically refreshes every 30 seconds using a `<meta http-equiv="refresh" content="30" />` in the HTML `<header>`.

## Issues
The data for the quotes is crowd-sourced. As such, there are errors. I also probably created some errors in my fumbling through conversion to JSON. Not every minute of the day has an associated quote. Contributions are WELCOME!

* Formatting of the time in the quotes:
  * The `object.descriptive` and `object.quote` *must* match.
  * For consistency, time should be 
    * `hh:mm a` format: 12:01 PM or 4:32 AM.
    * `hh a` format: 2 PM or 8 AM.
    * Since these are quotes, I've tried to match the original text, except for the time formats as noted above. Usually differences in time are due to translated text or copy/paste errors.
* Ambiguous or missing times:
  * A quote that mentions, "sometime around midnight" can be used to fill in missing times between 11:40 PM and 12:20 AM.
  * A quote that simply mentions, "early morning" or "late afternoon" can be used to fill in missing times.
  * PULL requests that insert ambiguous time quotes will take me time to approve.
* Punctuation and Quotations
  * Because of errors on my part and the original format of the dataset, there are ample punctuation, spacing, and quotation issues within the dataset.
    * There should be a space following commas:
      * "The time was ten-thirty but it could have been three in the morning, because along its borders, West Berlin goes to bed with the dark." is correct.
      * The time was ten-thirty but it could have been three in the morning,because along its borders,West Berlin goes to bed with the dark." is *not correct*.
    * Each period, even ellipses, should be followed by a space:
    * "\"It is eleven o'clock!Eleven o'clock, all but five minutes!\" \"But which eleven o'clock?\" \"The eleven o'clock that is to decide life or death!... He told me so just before he went.... He is terrible.... He is quite mad: he tore off his mask and his yellow eyes shot flames!...\"" is correct.
  * Punctuation goes INSIDE the quotes, with exceptions as noted here:
      * \"When,\" he asked with a smile, \"are you leaving for your date?\"
      * Commas used for separating the quote from the speaker's actions are sometimes split like this example.
      * There are many comma, semi-colon, and period errors from the original dataset.
  * Any spoken text should have double quotes surrounding the speech. \"Hello, World!\" is correct. 'Hello, World!' is not.
  * Quotes within quotes use single quotes. \"He said, 'Hello, World!'\" is correct.
  * Double quotes *MUST* be escaped. This is one that will actually break the JSON.
    * \"Go ahead, make my day.\" is correct.
    * "Go ahead, make my day." is not correct.
  * Single quotes and apostrophes *are not* escaped:
    * "'He's a charmer,' she thought, 'but it won't work on me.'" is correct.
    * "\'He's a charmer,\' she thought, \'but it won\'t work on me.\'" is not correct.
  * When in doubt, use the [Chicago Style Guide](https://owl.purdue.edu/owl/research_and_citation/chicago_manual_17th_edition/chicago_manual_of_style_17th_edition.html) as that is usually the standard for fiction/non-fiction writing.
  
PULL requests submitted for very few changes will likely be rejected for not being significant. I would _prefer_ edits cover at least a 30-minute period of the clock. However, if your PULL request handles a minor change (like adding spacing after semi-colons) for the entire clock, it will be approved.

## Improvements
I would like to add the following improvements to the code:

  * Format the time reference (using `object.descriptive`) inside of the `object.quote`.
    * Insert `<span id="descriptive"></span>` around the `object.descriptive` text in the `object.quote`. This could be done as an edit to the JSON file or a javascript function.
    * I do not want to use any additional javascript libraries.
  * Create a changeable site theme.
    1. A drop-down menu labeled "Theme"
    1. The menu-items will be the various themes available
    1. A Javascript function will modify a `<header>` tag to insert a new CSS style sheet
      1. Target is `<link href="" rel="stylesheet" id="theme" />`
      1. New stylesheet will control `color`, `background-color`, and `background-image` properties for the `body`, `title`, and `quoteArea` `div` elements.
    1. The existing `style.css` file will be modified to use CSS variables. The theme CSS file will contain the variables.
  1. Modify the Javascript to display a random matching quote, when more than one quote exists.
    1. Noon or midnight are two examples of times that have multiple quotes.
    1. Generate an empty array to assign each matching object
    1. Pick a random index and assign that object's values to the clock.
