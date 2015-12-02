---
layout: post
title: Using the Twilio lookup service to scrub your sample
---

As may of you know we’ve been working with the folks over at Twilio on our upcoming IVR service and to look at opportunities to configure our dialer to use their elastic SIP trunking service.  In our investigations, we encountered a great service for checking phone number details called [Twilio Lookup.](https://www.twilio.com/lookup) 

Twilio lookup provides several useful pieces of information, but given growing emphasis on dialing practices due to TCPA regulations, we’re most interested in determining the ‘type’ of number before diaing. When you call the service and provide a phone number, Twilio lookup will indicate whether the number is a landline, mobile, or VoIP. This can be very useful when looking to separate your dialing modes by phone type. 

We wrote a very simple script to demonstrate how to use this function that you can modify to fit your particular workflow. 

To get started, you’ll need a Twilio account and an associated Account SID and Authorization token. To find your credentials, follow the following information provided by Twilio, First log into your [Twilio Account](https://www.twilio.com/user/account). On the Dashboard there is a section labeled [API Credentials](https://www.twilio.com/user/account/settings#api-credentials). There you will find your Account SID and Auth Token. You'll need these to authenticate your request.”

Once your account is established and you have your credentials, you can got take a look here (add link) for a simple example on how to read a CSV file, call the lookup service, and report back the type of phone number for each record. Remember, there is a $0.005 charge for each number you lookup that will be billed to your Twilio account. 

This particular example demonstrates the use of this service to ‘pre-process’ all the records. However, this could easily be implemented to make the call to the Twilio service prior dialing, or at any point that makes sense for your particular workflow.

You can find the sample script on [github](https://github.com/survox/sampleScrub)
