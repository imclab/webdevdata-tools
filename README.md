# Tools to extract data form webdevdata.org

Small set of command line tools written in Go to help extracting data
from webdevdata.org.

## Installing the tools

**You can download binaries for Linux, Windows and Mac in the [GitHub releases
page][releases].**

## ```wdd_select [-atrs=attr1,attr2...] [CSS selector] [files]```

Searches for all tags matching the proviced ```CSS selector``` in
```file``` and prits a CSV with ```file,tag_name``` to ```STDOUT```.

Files to parse can be passed as arguments or via ```STDIN``` with one
file per line.

The ```-attrs``` option can be used to provide a comma separated list of
attributes to print in the CSV.

Examples:

Passing file as an argument and specifying attributes to print

```bash
$ wdd_select -attrs="class,id" "section, body > div" data_samples/forecast_io.html
data_samples/forecast_io.html,div,inner,""
data_samples/forecast_io.html,section,currently section,""
data_samples/forecast_io.html,section,next_hour section,""
data_samples/forecast_io.html,section,next_24_hours section,""
```

Passing files via ```STDIN```

```bash
$ find data_samples -name "*.html" | wdd_select "head, body"
data_samples/forecast_io.html,head
data_samples/forecast_io.html,body
data_samples/jimsmarketingblog_com.html,head
data_samples/jimsmarketingblog_com.html,body
```

## ```wdd_meta_names [file]```

Checks HTML meta tags from ```file``` and prints a CSV with
```file,meta_name``` to ```STDOUT```.

example:

```bash
$ wdd_meta_names data_samples/jimsmarketingblog_com.html
data_samples/jimsmarketingblog_com.html,description
data_samples/jimsmarketingblog_com.html,google-site-verification
data_samples/jimsmarketingblog_com.html,google-site-verification
data_samples/jimsmarketingblog_com.html,y_key
```

Generating CSV with all meta tag names from webdevdata.org crawl (using
GNU/Parallel instead of ```xargs``` to parallelize work):

```bash
$ find webdevdata.org-2013-10-30-231036 -name "*tml.txt" | parallel "wdd_meta_names >> meta_names.csv"
```
## ```wdd_html_manifest [file]```

Checks for html tag with manifest attribute from ```file``` and prints a CSV
with ```file,manifest_value``` to ```STDOUT```.

example:

```bash
$ wdd_html_manifest data_samples/forecast_io.html
data_samples/forecast_io.html,cache.desktop.manifest
```

## ```wdd_tag_count [file]```

Counts all HTML tags from ```file``` and prints a CSV with
```tag,count``` to ```STDOUT```.

example:

```bash
$ wdd_tag_count data_samples/jimsmarketingblog_com.html
data_samples/jimsmarketingblog_com.html,meta,13
data_samples/jimsmarketingblog_com.html,li,40
data_samples/jimsmarketingblog_com.html,footer,1
data_samples/jimsmarketingblog_com.html,script,15
data_samples/jimsmarketingblog_com.html,a,78
data_samples/jimsmarketingblog_com.html,option,64
data_samples/jimsmarketingblog_com.html,nav,1
data_samples/jimsmarketingblog_com.html,img,11
data_samples/jimsmarketingblog_com.html,input,4
data_samples/jimsmarketingblog_com.html,center,8
[...]
```

## Building tools

 1. ```go get github.com/webdevdata/webdevdata-tools```
 2. ```cd $GOPATH/src/github.com/webdevdata/webdevdata-tools```
 3. ```make all```
 4. Tools are in the build directory

You can use ```make release``` to generate cross-compiled binaries for Linux,
Windows and Mac.

[releases]: https://github.com/Webdevdata/webdevdata-tools/releases

