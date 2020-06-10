# Summary

The vulnerability is basically the lack of rate limit when Microsoft Teams Webhook is performing GET requests to load external images

# Descritpion

Microsoft Teams Incoming Webhook supports html, so it was possible to get external images using `<img src=""/>`, the browser tries to load the message and the backend performs a GET request trying to retrieve the image, so it was possible to replace the picture url to any url and perform GET requests to any website, with the lack of rate limit was possible to flood the website with GET requests.

## Steps to Reproduce: (Add details for how we can reproduce the issue)

1. Sign in on Microsoft Teams    
2. On 'MyTeam' > General, add a 'Incoming Webhook' Connector    
3. Copy the Webhook URL
4. Insert the URL into the Script
5. The script loads 140 broken images into the request `<img src="example.com?randomquery={0}"/>`
6. Every request must have a diferent query to load a 'different' image and perform a different GET Request
7. I send the request 20 times
8. When I open the teams app and the Teams channel loads, it performs all the GET Requests at the same time, causing a flood on the server receiving the requests, I can even send the request to various channels and click to load one after another.
9. I set up a simple Apache2 Server to test the requests and I was able to get more than 2600 requests in a couple seconds 
10. The IP of the request received is '52.114.128.37' and the User-Agent is 'Mozilla/5.0 (Windows NT 6.1; WOW64) SkypeUriPreview Preview/0.5'
    
## Supporting materials/ references:
    
* Screenshot 1 - Testing the flood on webhook.site
* Screenshot 2 - Screenshot of the Teams Screen after loading 'images'
* Apache2 Logs - access.log
* Exploit em python3 - exploit.py
