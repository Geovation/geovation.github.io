---
title: "Be Prepared"
meta: "A non-technical founders guide to the accounts and artifacts you'll have to create to bring your technical startup to life."
categories: Insights Startup
author: "Paul Nebel"
slug: "be-prepared"
---

As a non-technical founder of a technical startup it can be a confusing, worrying and downright difficult process to create all the accounts, documents, descriptions, images, logos and general stuff you need to create before you can bring your product to life.  This post is designed to let you know what you'll be most likely to need and why so that you can avoid delays and difficulties further down the line.

*NOTE: This document is not comprehensive and in no way constitutes a recommendation for any particular supplier or technology mentioned.*

All startups are different and it's impossible to provide generic advice that applies to everyone.  Like most things in life, what you will need depends on what you are doing.  Having said that, chances are that you'll need at least some of what is contained in this post.  Use this as a guide to the kind of things you'll need to do if you want your product or service to go live.

Buckle up, we have a long way to go and the road can get rocky!

## TLDR;
This is a long post by necessity; there's a lot to do to set up the technical environment for your startup.  If you just want a quick summary there is a [**Checklist**](#checklist-1) at the end of each section that lists the main actions to be undertaken.

# Code Repository & Version Control
All technical products and services require code.  This is the hundreds or thousands of lines of Javascript or Python or Node or whatever it is that your development team is using to create your business.  Even so-called *No Code* products create code - the only difference is that the code is being written by the product you're using and not by you.

### Code Repository
The most basic requirement of any and all technical products or services is a code repository.  In essence, a code repository is a file system that stores the digital files containing the code that constitutes your product or service.

Unless you're someone like Facebook or Google this will be a third-party, cloud-based service.  Some big companies like to host their code repository 'on premise' meaning that the servers running their code repository are hosted on physical machines somewhere in a building they own, but you aren't one of those yet so it's not even worth considering.

Most code repositories are associated with a particular [Version Control][versioncontrol]{:target="_blank"} system. Version control systems are responsible for managing changes to computer programs, documents, large web sites, or other collections of information. It is the mechanism by which your developers can create your product on their own laptops, independently of each other, and stitch the results together to make something that works (hopefully!).

### Version Control
Although I'm sure there will be some developers out there who disagree with me I'm going to make an exception to the rule I defined at the start of this post and say that the *only* version control system worth considering is [Git][git]{:target="_blank"}. Don't bother with anything else.  Where you do have a reasonable choice, however, is which Git-compliant repository you choose. The de-facto repository for Git is [GitHub][github]{:target="_blank"} but several others exist, the main competitor being [BitBucket][bitbucket]{:target="_blank"}.

Whichever repository you choose, make creating an account a priority because nothing much else can happen until you do.  Typically you'll need to create a team and invite your developers and other interested parties to it (who will, themselves, need logins to your repository of choice).

I've worked with several startups who have had the same issue, which is that they engaged a third-party software development company to create the initial version of their product.  In these cases, the third-party set up the code repository and essentially used access to the repository and the code it contained as a bargaining tool to force price increases.  If you are working with a third-party make sure you create the repository account yourself and add them as team members.  Also, make sure that payment is dependent upon *working* code being added to that repository.  That code belongs to you, it's what you've paid for, so make sure no-one can hold you to ransom when you try to access it.

### Checklist 1
> - Choose a Git-compliant cloud repository and create an account.
> - Create a team in your cloud repository and add your development team members to it (they will also require accounts with your cloud repository)
> - [Next Checklist](#checklist-2)

Writing, storing and managing code is one thing, running it somewhere where other people can access it is quite another.  For this you need hosting.

# Servers and Hosting
Any products that require a *backend* will need some sort of server.  Backends are used to run things like [APIs][api]{:target="_blank"} and [domain models][domainmodel]{:target="_blank"} that would either be too complex or insecure to run in an app.  Every website requires a server, even if it is just a [static][static]{:target="_blank"} marketing site. Even so-called [serverless][serverless]{:target="_blank"} code runs on a server, it just happens to be one you don't have to manage (much) yourself!

A cloud provider who runs your server is typically called a *host*.

There are a great many providers of cloud servers, including [Amazon Web Services (AWS)][aws]{:target="_blank"}, [Google Cloud][googlecloud]{:target="_blank"} and [Microsoft Azure][azure]{:target="_blank"} to name but three of the big ones. At Geovation we have a soft spot for [Heorku][heroku]{:target="_blank"}.  There are, of course, many others.  Most of them offer some kind of free tier for when you are getting started, but be aware that these free services are almost never suitable for production use due to their limited size and performance (they are free, after all) so make sure to check the costs you may incur when you go live.

Create an account with your server host and invite the team members responsible for deploying your product/service to this account.  Typically this will be done using the [Identity and Access Management (IAM)][iam]{:target="_blank"} system implemented by your host as this will allow you fine-grained control over who is allowed to do what.

### Checklist 2
> - Choose a cloud-based host for your servers (and possibly for ancilliary services - see below) and create an account.
> - Add your team members to this account using whatever IAM system is available, making sure to assign the correct permissions to each team member (do **not** just make everyone an administrator)
> - [Next Checklist](#checklist-3)

All hosts will associate a randomly generated URL with any servers you create.  Typically, these random URLs will be long, apparently illogical and almost impossible to remember.  In some cases, they may be subject to regular change without notice.  To overcome this, you need a domain.

## Domains, URLs and SSL Certificates
With the possible exception of self-contained apps, everyting you host will need a dedicated [domain name][domain]{:target="_blank"} and associated [URL(s)][url]{:target="_blank"} in order to be reachable.

### Domains & URLs
Choosing a suitable, available domain name is likely to feel like the hardest thing you've ever done! You can reasonably expect that every word of 10 letters or fewer has already been registered in some form or another. I've personally been looking for domain names in the past and have *made up* words which I've discovered have already been registered!

You may have to relinquish that `.com` domain in order to have the name you like because it has already been taken.  Be aware that there are people who make businesses out of speculatively registering domains, sitting on them and charging premium prices to sell them.  Given that there are billions of registered domains already the chances of getting the one you really want are slim to nothing, unless you have very deep pockets.

There are more domain name registrars than you can shake a stick at, just search for 'domain name' in Google and take your pick. Many of these registrars also offer hosting but in my experience it's better to host with one of the top 3 providers (AWS, Google Cloud, Azure) if you are writing your own code as they give you much more control over what can be done and have many more years experience at managing hosting, meaning that support is likely to be much better.

Most domain name providers offer at least one free email address with each domain together with the ability to purchase more email addresses.  Typically, these unmanaged email addresses can be forwarded to another account. Alternatively, you can choose an email address that is hosted and managed by the domain name provider.  Either way, you'll almost certainly need these email addresses for other accounts. Make sure your provider gives you access to your [Nameserver][nameserver]{:target="_blank"} and [DNS][dns]{:target="_blank"} records for your domain, as you'll also need this later.

### SSL Certificates
If you want to be searchable on Google and trusted by your customers your URLs will have to be secure (i.e. start with `https://`), which in turn means you'll need an [SSL Certificate][ssl]{:target="_blank"}.  Most domain name registrars provide the ability to purchase SSL Certificates although they rarely create these certificates themselves; they simply order them on your behalf from specialists like [GeoTrust][geotrust]{:target="_blank"} or [RapidSSL][rapidssl]{:target="_blank"}.  Check how much you are being charged by the domain name provider as you can buy SSL Certificates directly from these specialists if it is more cost effective.

You can also get free SSL Certificates from [Let's Encrypt][letsencrypt]{:target="_blank"} but be aware that these free certificates come with drawbacks. Fristly, they are only valid for 90 days (as opposed to 2 years for most paid certificates) so will need regular updating by you.  Secondly, they come with a lower protection level than paid certificates so may still be categorised as vulnerable in many web browsers.  Free SSL Certificates are very useful for development, however.

Just to make life a little more interesting, there are several [different types of SSL Certificate][differentssl]{:target="_blank"} you can choose from, each of which offers different protection and different costs. Essentially you have to choose from a **Single Domain** certificate (which applies to one domain and one domain only), a **Wildcard** certificate (which applies to one domain and any subdomains) or a **Multi-Domain** certificate (which is a combination of the above).  Choose wisely, as the costs can quickly add up to a significant amount.

### Checklist 3
> - Choose a cloud-based domain registrar and create an account.
> - Find a domain that is available and that you are happy with and buy it immediately before someone else does.
> - If possible, purchase some additional email addresses to associate with this domain.
> - Decide which type of SSL Certificate you require and start the process of purchasing it (it can take several days to come through)
> - [Next Checklist](#checklist-4)

It is highly unlikely that you'll be able to operate with a server alone.  Pretty much every tech product worthy of the name will need some additional ancilliaries.

# Ancilliaries
By themselves, servers do not generally provide enough functionality to create a comprehensive technical product or service.  You are very likely to require several ancilliary services including, but not limited to, the following:

 - Databases
 - Document storage
 - Email services
 - User management
 - Social login

Most cloud hosts will provide many of these services in addition to hosting  your servers. As with servers, most cloud hosts will provide free versions of these services with the similar caveats.

### Databases
There are many different databases, and many different types of database, and this post is not the place to discuss them all.  It is usual (but not required) to host your databases with the same provider as your servers unless you are using a particularly niche database. Check the different levels of paid database and look carefully at what you get from each level.  You could easily find that your database becomes the most expensive part of your ecosystem as your business grows.

**Don't forget about database backup and restore**.  You will probably need to roll back your database at some point in time and if the worst happens you need to know how to restore your database so practise now before it's too late and you realise you can't do it when you need to.

### Document Storage
It is highly likely that you'll want to host documents (including images) in a specialised Document Storage system like [AWS S3][awss3]{:target="_blank"}. These services are typically optimised to the hilt to ensure that your static assets are delivered as fast as possible and are always available. They also typically offer unlimited storage and global availability.

### Email Serivces
Email services like [AWS Simple Email Service (SES)][awsses]{:target="_blank"} will offer you a free *sandbox* version which limits the number of recipients and the frequency of sending of email.  Typically, you will have to add recipient email addresses manually before you can send emails to them from the sandbox.  This is to prevent these free services being used to create spam. Before you go live you will have to move 'out of the sandbox' and [into production][awssesproduction]{:target="_blank"}.

When doing so you have to state the use to which you will be putting the email account.  Make sure you tell the truth and make sure you stick to what you say otherwise you will very quickly find your email addresses blacklisted and your account suspended, meaning that you will be unable to send anything to anyone whether you need to or not. Being blacklisted by one email provider makes it quite likely that you will be rejected by other email providers. Email providers are terrified of being associated with spam and go to great lengths to prevent anything that looks remotely like spam coming from one of their accounts.

### User Management & Social Login
Please, please, please **do not write your own user management system**.  I see this time and again with technical startups and it is completely unnecessary as there are many good third-party user management systems available that will do this for you. Writing your own will suck up huge amounts of time and money.  If possible, use [Social Logins][sociallogin]{:target="_blank"} so customers can utilise existing accounts to access your product/services without having to create yet another login.

If you are including Social Logins you will have to create Developer accounts for any and all third-party sites you use.  To save time and hassle you should also create developer accounts with these services for your team members otherwise you'll spend an awful lot of time forwarding authentication keys and passwords to your developers as they test your product/service.

Much like Email Services, most social login providers will start you off with a sandbox test environment that you will have to upgrade in some way to make production ready.  Check the process for doing this for each provider and make sure you have the necesary artifacts available.

Be aware that, beginning with iOS 13, any iOS app that includes third-party authentication options must provide [Apple authentication][appleauthentication]{:target="_blank"} as an option in order to comply with App Store Review guidelines.

Given that [GDPR][gdpr]{:target="_blank"} is mandatory for European websites it is essential that you save only the absolute minimum of customer data necessary for the legitimate working of your product or service. Note that GDPR applies regardless of the UK's membership of the EU if you are providing products or services to EU members.

### Checklist 4
 > - Create any additional accounts (if necessary) for all ancilliary services you require.
 > - As with hosting, use the IAM system for these additional providers to create accounts for your team members.
 > - Buy, not build, a user management system.
 > - Create developer accounts for yourself and your team members for all social logins you are using (including Apple Authentication if you are using social login on iOS).
 > - Check the process for making ancilliary services like email and social login production ready and make sure you have all the necessary artifacts available
 > - [Next Checklist](#checklist-5)

# Apple & Google Develper Accounts
If you're making an app you'll need to enrol on the Apple & Google Developer programs in order to distribute your app. This is not strictly true for Google since it has a more open policy regarding app distribution but if you want your app to be discovered it's best to put it on Google Play.  For iOS apps, however, the Apple App Store is your only option (and **Apple will charge you 30% of everything you earn through the app** for the privilege!).

### Apple Developer Program
You must enroll in the [Apple Developer program][adp]{:target="_blank"} as an Organization (not as an  Individual). For UK businesses, this will typically require you to provide a copy of your [Memorandum and Articles of Association][maaa]{:target="_blank"} in order to prove your legal status.  To enroll in the Apple Developer program, you’ll need to set up an Apple ID and pay a $99/year fee. If you’re a nonprofit or government agency, Apple will waive your fee.

The process of enrolling can take several weeks, so the sooner you start the better it will be.

### Google Developer Program
Enrollment on the Google Developer program is much easier.  To enroll in the Google Play Developer program, you’ll need a Google account and to pay a one time $25 fee.

### Payment Provider
If you are planning to charge for your app, whether with a one-off fee or in-app purchases, you will need an online [Payment Service provider][payment]{:target="_blank"}.  Choose your provider and create an account.

Do not leave this to the last minute because you can't go live without it.

On Google Play, if you set the price of your app to *free* you **cannot change it to paid in the future** so make sure you know what you are going to charge before you publish your app to the store.

### Notifications
Decide if and how you will [communicate with your users in-app][notifications]{:target="_blank"}. Notifications are incredibly important in maintaining users.  It's hard enough getting users to download your app in the first place, [let alone use the app more than once][keepusers]{:target="_blank"}.  Notifications can be extremely useful in this situation but you have to decide on a strategy before it can be usefully implemented.

### Testing
In the vast majority of cases you will need a secure connection to test your app.  As discussed above, you can either do this with a free SSL Certificate and a lot of manual management or you can use a tunneling service like [Ngrok][ngrok]{:target="_blank"} to do it for you.  I use Ngrok every time.

### Privacy Policy, Terms & Conditions
If you collect any personal information from the users of your app you will need a [Privacy Policy][privacypolicy]{:target="_blank"} to comply with worldwide legislation.  Even if you don't directly collect personal information you may still need a privacy policy if you use third-party tools like Google Analytics to collect data on your behalf.

[Terms & Conditions of Use][tsandcs]{:target="_blank"} are not a legal requirement but they are strongly advised. You are also strongly advised to have both your privacy policy and Terms & Conditions of Use reviewed by a legal specialist. This may seem expensive, but the potential cost of not doing so could be orders of magnitude higher.

### Checklist 5
 > - If you intend to create an iOS app you need to create a company right now.
 > - You also need a publicly visible website to go with your company for iOS registration (this can be a simple marketing site, but you'll need something).
 > - Register with the Apple and Google Developer programs as soon as possible (since it can take some time for your Apple registration to be processed).
 > - Choose a payment provider and create an account.
 > - Decide what, how and how much customers are going to pay for your product/service.
 > - Decide whether you need a notification process and if so, how it will work.
 > - Create a free SSL Certificate or an Ngrok account for testing your apps.
 > - Create a Privacy Policy and Terms and Conditions of use and get them reviewed by a legal specialist.
 > - [Next Checklist](#checklist-6)
 
 
# One Final, VERY IMPORTANT Point
Do not ever, and I really mean *NOT EVER*, save usernames, passwords, Client IDs, Client Secrets or any other credentials you create during this process to cloud services which are not encrypted both in transit and at rest.

### Checklist 6 - Places NOT to store secure credentials
 > Do not save secure credentials to *ANY* service which is not encrypted both in transit and at rest or in any way publicly accessible. This includes all of the following, *none of which are truly secure*:
 > - Slack
 > - Google Docs
 > - Your code repository
 > - Dropbox
 > - Gmail
 
Be very careful about who, in the team, you share your credentials with.  Remember: [**90 percent of data breaches are caused by human error**][databreach]{:target="_blank"}.

Don't say I didn't warn you!
 
# Summary
That's an awful lot of information and a bucket-load of things to do.  Exactly what needs to be done and which mechanisms/suppliers you will use will depend entirely on what you are trying to do but hopefully this guide has given you a template for starting your technical business in a way that lets your developers develop while you concentrate on building your business.

  [versioncontrol]: https://en.wikipedia.org/wiki/Version_control
  [git]: https://git-scm.com/
  [github]: https://github.com/
  [bitbucket]: https://bitbucket.org/product
  [api]: https://en.wikipedia.org/wiki/Application_programming_interface
  [domainmodel]: https://en.wikipedia.org/wiki/Domain_model
  [static]: https://en.wikipedia.org/wiki/Static_web_page
  [serverless]: https://en.wikipedia.org/wiki/Serverless_computing
  [aws]: https://aws.amazon.com/
  [googlecloud]: https://cloud.google.com/
  [azure]: https://azure.microsoft.com/en-gb/
  [heroku]: https://www.heroku.com/
  [iam]: https://en.wikipedia.org/wiki/Identity_management
  [domain]: https://en.wikipedia.org/wiki/Domain_name
  [url]: https://en.wikipedia.org/wiki/URL
  [ssl]: https://www.cloudflare.com/learning/ssl/what-is-an-ssl-certificate/
  [nameserver]: https://en.wikipedia.org/wiki/Name_server
  [dns]: https://en.wikipedia.org/wiki/Domain_Name_System
  [geotrust]: https://www.geotrust.com/
  [rapidssl]: https://www.rapidssl.com/
  [letsencrypt]: https://letsencrypt.org/
  [differentssl]: https://www.cloudflare.com/en-gb/learning/ssl/types-of-ssl-certificates/
  [awsses]: https://docs.aws.amazon.com/ses/latest/DeveloperGuide/Welcome.html
  [awssesproduction]: https://docs.aws.amazon.com/ses/latest/DeveloperGuide/request-production-access.html
  [awss3]: https://docs.aws.amazon.com/AmazonS3/latest/gsg/GetStartedWithS3.html
  [sociallogin]: https://en.wikipedia.org/wiki/Social_login
  [appleauthentication]: https://developer.apple.com/sign-in-with-apple/
  [gdpr]: https://en.wikipedia.org/wiki/General_Data_Protection_Regulation
  [adp]: https://developer.apple.com/programs/enroll/
  [maaa]: https://www.gov.uk/limited-company-formation/memorandum-and-articles-of-association:
  [gdp]: https://play.google.com/apps/publish/signup/
  [payment]: https://en.wikipedia.org/wiki/List_of_online_payment_service_providers
  [notifications]: https://medium.com/@Imaginovation/app-development-decisions-push-notifications-vs-in-app-messaging-5d7ac1e56233
  [keepusers]: https://techcrunch.com/2013/03/12/users-have-low-tolerance-for-buggy-apps-only-16-will-try-a-failing-app-more-than-twice/?utm_source=feedburner&utm_medium=feed&utm_campaign=Feed%3A+Techcrunch+%28TechCrunch%29
  [ngrok]: https://ngrok.com/
  [privacypolicy]: https://www.privacypolicies.com/blog/mobile-apps-privacy-policy/
  [tsandcs]: https://www.privacypolicies.com/blog/terms-conditions-mobile-app/
  [databreach]: https://www.techradar.com/uk/news/90-percent-of-data-breaches-are-caused-by-human-error
  
  
  
  

 