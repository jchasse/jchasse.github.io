---
layout: post
title:      "Running my first program // CLI App development"
date:       2020-08-01 21:59:39 +0000
permalink:  running_my_first_program_cli_app_development
---

Howdy! (having lived 1/3 of my life in Texas...this greeting tends to grow on you)

I just completed my first cli app development project and I am wiriting about my wins, challenges, and opportunities relating specifically to the coding of this project.

**Biggest Win -> Connecting to a third party data source via API**

   Being a person that consistently sees opportunity for efficiency and improvement, this was incredibly rewarding to write code that can connect with a third party URL to request specific information that my code/user is requesting. Here is how I went about incorporating an API into my first CLI app.
	 
* 	 Defined a new class called API to seperate the concerns of the code. Other classes included a controller that was focused on the interface with the user (utilizing puts and gets) and the model class that created and kept track of the instances.
* 	 For these API get requests, I decided to utilize the gem 'HTTParty' to simplify the process and return the data in the format my ruby app could use. 'HTTParty' allows us to send the GET request, retrieve XML object and format it into nested arrays in which my code was expecting to navigate.
* 	 The responde from calling the URL and utilizing the gem was then sent back to the CLI class for handling and assigning to a new instance.

```
class API

    def self.get_animal_tsn(animal)
        url = "http://www.itis.gov/ITISWebService/services/ITISService/searchByCommonName?srchKey=#{animal}"
        response = HTTParty.get(url) 
    end

    def self.get_animal_details_by_tsn(detail_type, tsn)
        url = "http://www.itis.gov/ITISWebService/services/ITISService/get#{detail_type}FromTSN?tsn=#{tsn}"
        response = HTTParty.get(url) 
    end
end
```

**Biggest Challenge -> Handling API hashes and arrays**
    One of the areas that had me stumped for a bit was how to handle the API response. The challange came due to the API slightly changing the format of the data depending on the volume of information being returned.
		
* 		Most frequently, the data that I was calling required multiple hashes inside of an array to be returned
* 		Infrequently, there would be a call made that returned a single hash.....not in an single element array
* 		To solve for this variability, I needed to write an if statement utilizing the class of the return value to seperate how to handle between an array and a hash. 
* 		My learning was that by using the .class method, I was able to determine if my GET request returned an Array or a Hash

```
def print_selected_details(detail_select, animal)
        if detail_select == "Scientific Name"
            puts "\nScientific Name: #{animal.sci_name}"
        
        elsif detail_select == "Full Hierarchy"
            puts "\n"
            if animal.full_hier.class == Hash
                puts "#{animal.full_hier["rankName"]}: #{animal.full_hier["taxonName"]}"
            elsif animal.full_hier.class == Array
                animal.full_hier.each do |hier|
                    puts "#{hier["rankName"]}: #{hier["taxonName"]}"
                end
            else
                puts "\nThis input resulted in an error in pulling from our database."
            end
 
        elsif detail_select == "Publications"
            if animal.publications.class == Hash
                puts "\nPublication: #{animal.publications["pubName"]}"
                puts "Author: #{animal.publications["referenceAuthor"]}"
                puts "Title: #{animal.publications["title"]}"
                puts "Date: #{animal.publications["actualPubDate"]}"                

            elsif animal.publications.class == Array
                animal.publications.each do |pub|
                    puts "\nPublication: #{pub["pubName"]}"
                    puts "Author: #{pub["referenceAuthor"]}"
                    puts "Title: #{pub["title"]}"
                    puts "Date: #{pub["actualPubDate"]}"
                end
            else
                puts "\nNo publications recorded in our database yet"
            end
        else
            self.invalid_input
        end
        what_next?(animal)
    end
```

**Biggest Opportunity -> Added feature and functionality

One thing I am learning fast is the amount of functionality and features you add can be vast. My mind quickly ventures down rabbit holes on what would be nice to have vs solving the problem with the most direct path and most time (cost) effective route.**

Having some experience in finance and P&L management, I believe this skill of descerning must have and nice to have functionality will be cirtical to future product management decision making.
