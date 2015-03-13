# Goals for Beta "release" #

After the following functions/issues are fixed, we will consider the project to be in beta.

  * In-page links should be working (currently works in everything except IE6)
    * Running into issues with in-page links and the proxy (?) on [cnn (example)](http://www.cnn.com/2009/POLITICS/03/24/obama.news.conference/index.html).
  * Highlight chunks of text as read (working for most chunks, but work on smaller chunks in some instances?)
  * Handle frames and iframes
    * [frame example](http://www.martinish.com/folio/linkFrame/?url=www.schoolonthegreen.com&return=/folio/web)
    * [iframe examples](http://www.dyn-web.com/tutorials/iframes/)
    * [iframe test page](http://sp1ral.com/tests/iframe4.html)
    * Text content of an iframe, e.g., `<iframe ...>text content</iframe>` causes an error for the spotlighter becaues it is not visible yet it tries to bring it into focus. Crashes.
  * Ensure "find" is working
  * Magnified view (Jeff checked this in. Test that it works across platforms)
  * If something is hidden, don't read it aloud.
  * Style of the "interface" - keep things together (bunched) and readable--all text in highlight. javascript->css.

## Testing before/for release ##

We want to test WA on the following set of sites/applications:

  * [google](http://www.google.com/)
  * [gmail](http://gmail.com/)
  * [university of washington](http://www.washington.edu/)
  * [facebook](http://facebook.com)
  * [bookshare](http://www.bookshare.org/)
  * [blackboard/moodle](http://www.blackboard.com/)-- not really a good test page?
  * [espn](http://espn.go.com/)
  * [nytimes](http://nytimes.com/)
  * [W3C/WAI](http://www.w3.org/WAI/)
  * [iframe test page](http://sp1ral.com/tests/iframe4.html)
  * [complex table test page](http://sp1ral.com/tests/flood-data-final.html)
  * [visibility test](http://sp1ral.com/tests/visibility.html)
  * [twitter](http://twitter.com) hangs, loads nothings. Find a way to fail gracefully.
  * others?

Platforms
  * IE6, IE8, Firefox3 (win/mac), Safari (mac)

Key combinations
Test all combinations listed in keyboard.js (update list of commands on "welcome to anywhere") and document for in a matrix for each page/platform combination
  * ctrl--stop talking
  * tab--next active element
  * ctrl forward slash--show list of keyboard shortcuts
  * esc--??
  * shift tab--previous active element
  * ctrl a--??
  * alt leftarrow--back one page?
  * alt rightarrow--forward one page?
  * ctrl n--??
  * ctrl l--move focus to location input element
  * ctrl tab & shift ctrl tab--(in there twice up here and last)
  * ctrl r--move to next row in a table
  * ctrl f--move focus to find input element
  * ctrl d--move to next column in a table
  * ctrl t--move to next table
  * ctrl shift t--move to previous table
  * ctrl h--next heading
  * ctrl shift h--previous heading
  * ctrl shift f5--??
  * ctrl shift f6--??
  * ctrl shift f7--reset timing array?
  * ctrl shift r--previous table row
  * ctrl shift d--previous table column
  * ctrl p--next paragraph
  * ctrl i--next input element
  * ctrl shift i--previous input element
  * ctrl 6--??
  * ctrl 7--??
  * ctrl 8--??
  * pagedown--read continuously from the current position
  * home--read continuously, starting over from the beginning of the page
  * ctrl shift r--reload?
  * ctrl shift tab--shift forward tabs (ala browser functionality)
  * ctrl tab--backwards through tabs
  * arrow down--read the next element
  * arrow up--read the previous element

Unit Testing
Check out the unit tests and update as needed.

## Feedback questions for beta ##
  * How do you feel about the amount of space taken up by the "interface at the top?"