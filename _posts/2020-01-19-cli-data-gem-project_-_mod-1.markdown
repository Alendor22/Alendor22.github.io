---
layout: post
title:      "CLI-Data-Gem-Project - Mod-1"
date:       2020-01-19 22:34:42 -0500
permalink:  cli-data-gem-project_-_mod-1
---

     For the module 1 final project I was required to build a CLI app that utilized either scrapping a website for data, or obtaining data from external API.  While both methods have their own challenges and require the use of separate gems to gather the data (Nokogiri for scraping data from websites, and Rest-Client for obtaining data from an external API), I chose the later - an API.  I will discuss the difficulties as well as the elation from engaging in this endeavor.  The first thing you do when you start to build the CLI-Data-Gem project is "bundle gem install <file_name>" in the directory of choice in the terminal.  This will generate a scafolding file structure to begin coding your project, you still have to create the files and name them according to the models you have, but its a huge start.  After you've done the necessary file building you may have to add more files as you conceptualize how your models will enteract with each other.  This is a vital part of production that would suit you best to whiteboard out.  I did the conceptualization in my project in my head and had to delete unnecessary files that I believed I needed initialy, and add one or two that it turned out that I needed, but as I hadn't coded anything in them it was not a problem.  Word to the wise - whiteboard your idea out!  
		 
		 The next thing you should do is install all the required gems, like so gem install rest-client, and json.  I used TTY-Prompt to control the output of the data to my CLI application so you'd also have to gem install tty-prompt.  My API required a API-Token so in order to include the API-Token in my code with out it being pushed to github I had to do the following.  To include the API-Token and keep it hidden per instructions form the API issuers docs I had to gem install dotenv.  Then I had to require 'dotenv/load' in my environment file at the top, followed by the affore mentioned gems and pry so you can place a binding.pry in any file and debug and inspect data.  Once you have all the required gems and your API-Token in the right place you can begin entering your code.  The first thing I did was create my API search method using `RestClient.get` to request data from my `BASE_URL`, which was a class constant so if I needed to access it across classes I could.  The parameters for making a `RestClient.get` request were as follows `response = RestClient.get(BASE_URL+"stock/#{stock_input}/quote/", {params: {token: ENV["API_TOKEN"]}})` .  I guess now would be a good time to mention that my project was based on getting stock quote data from my external API `https://cloud.iexapis.com/stable/`.  Once the data was obtained it had to be parsed as it was in the form of a JSON (JavaScript Object Notation), so I called the following method `quote_hash = JSON.parse(response.body)` on the response I obtained from the API which was saved as a variable (response) and saved that data to its own variable as well as you can see I called that variable quote_hash.  once that was completed I instantiate a new stock object with the follow `stock_data = Stocks.new(quote_hash)` and pass quote_hash obtained earlier as an argument to the `Stock.new`.  So the entire search method ininside the class in code is as follows:

```
class StocksImporter

  BASE_URL = "https://cloud.iexapis.com/stable/"

  def stock_search(stock_input)
    response = RestClient.get(BASE_URL+"stock/#{stock_input}/quote/", {params: {token: ENV["API_TOKEN"]}})
    quote_hash = JSON.parse(response.body)
    stock_data = Stocks.new(quote_hash)
  end

end
```
After you enter a stock to search for the above is executed, but not before I validate the stock symbol that was entered using the following method:

```
def stock_exists(symbol_input)
    @@stock_list.find { |stock| stock[0] == symbol_input.upcase }
    
  end
```
     I obtained a CSV file from the web with over 6900 currently traded stcoks.  Then a new stock object is instantiated using the following attribute accessor `attr_accessor :companyName, :symbol, :latestPrice, :previousClose, :primaryExchange, :latestTime, :change, :week52High, :week52Low`.  Once I have instantiated a new stock object it is initialized with the above properties and only the above mentioned attributes, to do that I utilize mass-assignment, what a great tool!  See below for the initialize method in the Stocks class 
```
def initialize(attributes)
    attributes.each { |key, value| self.send(("#{key}="), value) if self.respond_to?("#{key}=")}
    self.save
  end
```
     There are a few methods that I would consider run of the mill methods that perform sorts, finds, and collects on the data in the data set.  I hope you enjoyed reading this blog post as much as I enjoyed writing it.  Thank you for reading "Stay positve, motivated and keep coding!"
