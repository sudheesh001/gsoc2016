---
layout: post
title:  "Loklak API SDK Now Supports GoLang"
date:   2016-06-06 00:18:23 
categories: loklak
---
The Go programming language is quite a recent language. It’s a statically typed and compiled language un like Python or other scripting languages, It has a greater memory safety compared to C, supports garbage collection and an inbuilt support to make http and network requests using the `"net/http"` and `"net/url"` packages that are present in it. Go is scalable to very large systems like Java and C++. It also makes things productive and is easily readable because of the far lesser number of keywords that it has.

<img src="{{ site.baseurl }}/img/loklak-golang.png" alt="Loklak GoLang" width="100%">

Some of the key things that we notice with Golang coming from a C/C++ background is that there’s nothing called a class. Wait, what ? Exactly, you read it right, Go considers class to be mentioned as a struct . So the loklak object data structure which we will be using throughout golang’s support for the various API requests is as follows

{% highlight go %}
import (
    "encoding/json"
    "fmt"
    "net/http"
    "net/url"
    "os"

    "github.com/hokaccha/go-prettyjson"
)
{% endhighlight %}

So in golang, it’s recommended to have the built in packages first followed by the libraries that you’re using remotely, we’re using the pretty print json library which is at `github.com/hokaccha/go-prettyjson`. So since we follow the DRY (Don’t Repeat Yourself) method, we write a public function called getJSON as follows

{% highlight go %}
func getJSON(route string) (string, error) {
	r, err := http.Get(route)
	if err != nil {
		return "", err
	}
	defer r.Body.Close()

	var b interface{}
	if err := json.NewDecoder(r.Body).Decode(&b); err != nil {
		return "", err
	}
	out, err := prettyjson.Marshal(b)
	return string(out), err
}
{% endhighlight %}

If you’re coming from a C++/C background you’d notice something really odd about this, the return types need to be mentioned in the function header itself, so a function func getJSON(route string) (string, error) takes a string by the variable route as input and returns two values (string, error) as the return types. The above code takes the request URL and returns the corresponding JSON response.

Golang methods are generally not a very preferred type in such REST based scenario API development like that of Loklak, hence most of the queries can be directly made using functions. But we initially have a function for a method where we can set the loklak server URL to take.

{% highlight go %}
// Initiation of the loklak object
func (l *Loklak) Connect(urlString string) {
	u, err := url.Parse(urlString)
	if (err != nil) {
		fmt.Println(u)
		fatal(err)
	} else {
		l.baseUrl = urlString
	}
}
{% endhighlight %}

So this takes a string urlString as an parameter and creates a method called Connect() by using a loklak Object, this updates the base URL field of the loklak object. This is obtained as follows in the main package.

```
loklakObject := new(Loklak)
loklakObject.Connect("http://loklak.org/")
```
The Go language has built-in facilities, as well as library support, for writing concurrent programs. Concurrency refers not only to CPU parallelism, but also to asynchrony: letting slow operations like a database or network-read run while the program does other work, as is common in event-based servers. These could prove to be very useful in building apps with Go using loklak and to use loklak data, the high parallelism can be very useful for developers using data from loklak and building applications based on this.

Some very interesting things in golang is that golang doesn't support function overloading or default parameters, hence the search API which was implemented using default parameters in PHP and Python can’t be implemented that way in Go. This has been tackled by using the search() function and prepackaging the request to be made to it as a loklak object.

{% highlight go %}
// Search function is implemented as a function and not as a method
// Package the parameters required in the loklak object and pass accordingly
func search (l *Loklak) (string) {
	apiQuery := l.baseUrl + "api/search.json"
	req, _ := http.NewRequest("GET",apiQuery, nil)

	q := req.URL.Query()
	
	// Query constructions
	if l.query != "" {
		constructString := l.query
		if l.since != "" {
			constructString += " since:"+l.since
		}
		if l.until != "" {
			constructString += " until:"+l.until
		}
		if l.from_user != "" {
			constructString += " from:"+l.from_user
		}
		fmt.Println(constructString)
		q.Add("q",constructString)
	}
	if l.count != "" {
		q.Add("count", l.count)
	}
	if l.source != "" {
		q.Add("source", l.source)
	}
	req.URL.RawQuery = q.Encode()
	queryURL := req.URL.String()
	out, err := getJSON(queryURL)
	if err != nil {
		fatal(err)
	}
	return out
}
{% endhighlight %}

To use this search capability, one needs to create and package the requested parameters and then call this function with the loklak query.

{% highlight go %}
func main() {
	loklakObject := new(Loklak)
	loklakObject.Connect("http://loklak.org/")
	loklakObject.query = "fossasia"
	loklakObject.since = "2016-05-12"
	loklakObject.until = "2016-06-02"
	loklakObject.count = "10"
	loklakObject.source = "cache"
	searchResponse := search(loklakObject)
	fmt.Println(searchResponse)
}
{% endhighlight %}

The golang API has a great potential in aiding loklak server and being used by applications running golang and looking to build highly scalable and high performance applications using loklak. The API is available on [github](https://github.com/loklak/loklak_api_go), feel free to open an issue in case you find a bug or need an enhancement.
