# Exo2twitter
Activité 2 Twitter

Gardez bien le résultat de cette activité, vous serez amenés à le partager dans le prochain chapitre !


Réponse :
The Search API
The Twitter Search API is part of Twitter’s v1.1 REST API. It allows queries against the indices of recent or popular Tweets and behaves similarily to, but not exactly like the Search feature available in Twitter mobile or web clients, such as Twitter.com search.
Before getting involved, it’s important to know that the Search API is focused on relevance and not completeness. This means that some Tweets and users may be missing from search results. If you want to match for completeness you should consider using a Streaming API instead.
A detailed reference on this API endpoint can be found at GET search/tweets.

How to build a query
The best way to build a query and test if it’s valid and will return matched Tweets is to first try it at twitter.com/search. As you get a satisfactory result set, the URL loaded in the browser will contain the proper query syntax that can be reused in the API endpoint. Here’s an example:
1.	We want to search for tweets referencing @twitterapi account. First, we run the search on twitter.com/search
2.	Check and copy the URL loaded. In this case, we got: https://twitter.com/search?q=%40twitterapi
3.	Replace “https://twitter.com/search” with “https://api.twitter.com/1.1/search/tweets.json” and you will get: https://api.twitter.com/1.1/search/tweets.json?q=%40twitterapi
4.	Execute this URL to do the search in the API
Please note that now API v1.1 requires that the request must be authenticated, check Authentication & Authorization documentation for more details on how to do it. Also note that the search results at twitter.com may return historical results while the Search API usually only serves tweets from the past week.

Query operators
The query can have operators that modify its behavior, the available operators are:
Operator	Finds tweets…
	
watching now	containing both “watching” and “now”. This is the default operator.
“happy hour”	containing the exact phrase “happy hour”.
love OR hate	containing either “love” or “hate” (or both).
beer -root	containing “beer” but not “root”.
#haiku	containing the hashtag “haiku”.
from:alexiskold	sent from person “alexiskold”.
to:techcrunch	sent to person “techcrunch”.
@mashable	referencing person “mashable”.
superhero since:2015-07-19	containing “superhero” and sent since date “2015-07-19” (year-month-day).
ftw until:2015-07-19	containing “ftw” and sent before the date “2015-07-19”.
movie -scary :)	containing “movie”, but not “scary”, and with a positive attitude.
flight :(	containing “flight” and with a negative attitude.
traffic ?	containing “traffic” and asking a question.
hilarious filter:links	containing “hilarious” and linking to URL.
news source:twitterfeed	containing “news” and entered via TwitterFeed
Please, make sure to URL encode these queries before making the request. There are several online tools to help you to do that, or you can search at twitter.com/search and copy the encoded URL from the browser’s address bar. The table below show some example mappings from search queries to URL encoded queries:
Search query	URL encoded query
	
#haiku #poetry	%23haiku+%23poetry
“happy hour” :)	%22happy%20hour%22%20%3A%29
Note that the space character can be represented by “%20” or “+” sign.
Additional parameters
There is a set of additional parameters that allows a better control of the search results. The GET search/tweets documentation has detailed information about the usage of the parameters, this section will only give a brief description of their capabilities:
•	Result Type: just like twitter.com/search results, the result_type parameter allow to choose if the result set will be represented by recent or popular Tweets, or even a mix of both.
•	Geolocalization: The search operator “near” isn’t available in API, but there is a more precise way to restrict your query by a given location using the geocode parameter specified with the template “latitude,longitude,radius”, for example, “37.781157,-122.398720,1mi”. When conducting geo searches, the search API will first attempt to find tweets which have lat/long within the queried geocode, and in case of not having success, it will attempt to find tweets created by users whose profile location can be reverse geocoded into a lat/long within the queried geocode, meaning that is possible to receive tweets which do not include lat/long information.
•	Language: the lang parameter restricts tweets to the given language.
•	Iterating in a result set: parameters such count, until, since_id, max_id allow to control how we iterate through search results, since it could be a large set of tweets. The Working with Timelines documentation is a very rich and illustrative tutorial to learn how to use these parameters to achieve the best efficiency and reliability when processing result sets.
Rate limits
The GET search/tweets is part of the Twitter REST API 1.1 and is rate limited similarly to other v1.1 methods. See REST API Rate Limiting in v1.1 for information on that model. At this time, users represented by access tokens can make 180 requests/queries per 15 minutes. Using application-only auth, an application can make 450 queries/requests per 15 minutes on its own behalf without a user context.
Best practices
•	Ensure all parameters are properly URL encoded.
•	Limit your searches to 10 keywords and operators.
•	Queries can be limited due to complexity. If this happens the Search API will respond with the error: {"error":"Sorry, your query is too complex. Please reduce complexity and try again."}.
•	The Search API is not complete index of all Tweets, but instead an index of recent Tweets. At the moment that index includes between 6-9 days of Tweets.
Example searches
When you are following an event that’s currently happening, you would be interested in search for recent tweets using the event hashtag:
•	You want: recent tweets that contain the hashtag #superbowl
•	Your search URL is: https://api.twitter.com/1.1/search/tweets.json?q=%23superbowl&result_type=recent
When you want to know what tweets are coming from a specific location, with a specific language:
•	You want: all recent tweets done in Portuguese, near Maracanã soccer stadium in Rio de Janeiro
•	Your search URL is: https://api.twitter.com/1.1/search/tweets.json?q=&geocode=-22.912214,-43.230182,1km&lang=pt&result_type=recent
When you want the most popular tweets of a specific user using a hashtag:
•	You want: popular tweets from @Cmdr_Hadfield mentioning the hashtag #nasa
•	Your search URL is: https://api.twitter.com/1.1/search/tweets.json?q=from%3ACmdr_Hadfield%20%23nasa&result_type=popular


