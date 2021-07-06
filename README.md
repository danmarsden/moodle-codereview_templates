# Templates for reporting common issues during a Moodle plugin review 
When using github you can save these into your github account as "saved replies" to make it easy.

## Consider adding github action support
Some of the tests run by the Moodle.org plugins db can be run via github actions on each commit in your github repo. Enabling this helps you to make sure future changes to your plugin will continue to follow the guidelines.

the short version - grab this file:
https://github.com/moodlehq/moodle-plugin-ci/blob/master/gha.dist.yml
rename it as ci.yml and put into the directory .github/workflows within tyour project eg:
https://github.com/danmarsden/moodle-mod_attendance/blob/main/.github/workflows/ci.yml

then on every commit you make to github it will fire off a request to run the tests and will give you traffic lights beside each commit and generate a report.

## styles.css not specific enough
in your styles.css you define a number of generic styles that could clash with core moodle code or other locations where simliar names are used. 

Moodle helpfully adds a number of classes to the body tag based on the path that you can use such as:
eg if you are have a file in mod/assign you would see the following class added to the body tag
path-mod-assign
so if you have an item with the class "filething" on the page you would target it like:
```
.path-mod-assign .filething {
   color: red;
}
```

Please make sure your css classes in styles.css are specific enough so they cannot clash with other core code.

## Incorrect repository name
Your repository name should use the standard frankenstyle Moodle naming convention which makes it easy for moodle admins to identify your plugin.

the correct format to use is moodle-{frankenstylepluginname}
eg if your plugin was "mod_attendance" the correct name to use would be: "moodle-mod_attendance"

Github maintains redirects from your old name to the new one which makes this very easy - just hit the "settings" tab on your github repository and the rename repository option should appear near the top.

## Missing thirdpartylibs.xml
When including an external library in your plugin, you must include a thirdpartylibs.xml file that includes the name, location and license of the library.
More information on this is here:
https://docs.moodle.org/dev/Plugin_files#thirdpartylibs.xml
And here:
https://docs.moodle.org/dev/Plugin_with_third_party_libraries

One of the other advantages of using this is that Moodle's codechecker automatically ignores any files included in the location specificed in the thirdpartylibs.xml file.

Please note - this is typically a blocker for approval in the plugins db.

## Please use core curl class
Please don't initate curl_init() directly but use Moodle's own "curl" class which is a wrapper on PHP's curl functions.

Many organisations put their moodle site behind a web proxy and the internal curl wrapper takes care of this for you.

## Camel case
Moodle uses sentence case - please check your lang strings as some use camel case.

for example strings like:
$string['mystring'] = "My String";
should be:
$string['mystring'] = "My string";

Note: this is not a blocker for approval in the plugins db.

## Incorrect css location
Plugins should store their css within a styles.css file within the root of the plugin folder - this allows Moodle to cache the css correctly - if you store it in the styles.css file it will get picked up automatically and you don't need to include it manually in the page.

just make sure that anything you add to styles.css is really specific so that anything you add cannot clash with other areas of Moodle.

Moodle helpfully adds a number of classes to the body tag based on the path that you can use such as:
eg if you are have a file in mod/assign you would see the following class added to the body tag
path-mod-assign
so if you have an item with the class "filething" on the page you would target it like:

.path-mod-assign .filething {
   color: red;
}
you also get extra specific tags around blocks etc.

note this is not a blocker for approval.

## Invalid install.xml file
The install.xml is no longer valid and was either created for an old version of Moodle or has been handwritten.

This needs to be fixed using the Moodle xmldb editor (under admin > development > XMLDB.)

Please hit the "load" link for your plugin install.xml file in the moodle xmldb editor, and then make Moodle think there is an update to the file (easiest way to do this is to change the description of a table - add a full stop or something similar.) Then get XMLDB to "Save" the updated file which will remove all the old invalid xml from your file.

## String concatenation in lang
string concatentation in the lang packs is not allowed - it causes problems for the Moodle translation tool

eg:
$string['test'] = "sometext"."somemoretext";
or
$string['test'] = "sometext".$string['someotheritem'];

each string must be contained within one set of quotes eg:
$string['test'] = "sometext somemoretext";

## Missing backup api
Moodle activity modules must implement the backup api so that they are included in the course backup/restore process.

This is a blocker for approval in the plugins db.

## Missing privacy api - null provider
Moodle uses a privacy API for GDPR compliance to allow plugins to specify how they deal with user data. Your plugin doesn't appear to store any user data so you should be able to implement the simple null provider class.

Sites that use continuous integration processes will not be able to use your plugin because Moodle runs unit tests which check to see if all extra plugins include the privacy class.

More information on the privacy class is here:
https://docs.moodle.org/dev/Privacy_API

## Missing privacy api - user data.
Moodle uses a privacy API for GDPR compliance to allow plugins to specify how they deal with user data. Your plugin stores user data in a number of tables which will need to be included in the privaci api classes.

Sites that use continuous integration processes will not be able to use your plugin because Moodle runs unit tests which check to see if all extra plugins include the privacy class.

More information on the privacy class is here:
https://docs.moodle.org/dev/Privacy_API

