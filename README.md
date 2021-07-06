# Templates for reporting common issues during a Moodle plugin review - You can save these into your github account as "saved replies" to make it easy.

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

## Please use core curl class
Please don't initate curl_init() directly but use Moodle's own "curl" class which is a wrapper on PHP's curl functions.

Many organisations put their moodle site behind a web proxy and the internal curl wrapper takes care of this for you.

