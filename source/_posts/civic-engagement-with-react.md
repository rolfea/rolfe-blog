---
title: Civic Engagement With React
url: 36.html
id: 36
categories:
  - Uncategorized
date: 2017-07-14 13:04:19
tags:
---

I built [You Work For Me](https://you-work-for-me.netlify.com/) as part of the last quarter of my bloc.io program. The curriculum covers the Ruby/Rails and JavaScript/AngularJs stacks, but, having not had a great time with AngularJs, I was interested in exploring something new. My mentor suggested I try React.

### The Initial Idea

This time coincided with the Fall 2016 elections in the US, and I felt motivated to create something to encourage people to engage more in our political processes. I decided to build something that makes it easier to contact your congressional representatives. Below, I describe in more detail what my site, named You Work For Me, does and how I put it together. I finish with some ideas about the direction I’d like to take future development. The basic form of the app is as follows: a user navigates to the site and is presented with the contact information of their three congressional representatives. While simple, this foundation was easy to understand so that I was able to focus on learning React as well, and contains a basic set of features that I can augment in the future. The way YWFM gets and presents the congressional data is relatively straightforward. First, it takes the latitude and longitude of the user from the geolocation object in the browser, then sends an API request to the Sunlight Foundation’s Congress API, and finally, displays that data in a useful format to the user.

### Some Details

I access the user’s location when they first navigate to the site. The WebAPI has documentation on how the geolocation object behaves, but the basics are pretty straight forward. You ask for the latitude and longitude, the browser then asks the user if that is ok, and if they accept, an asynchronous request is made which returns an object that contains that information. I used a great library to modify the geolocation behavior into something more React savvy. [React-Geolocated](https://github.com/no23reason/react-geolocated) allows you to wrap an existing component, passing the geolocation details as props. This makes accessing and reasoning with the latitude and longitude as simple as

    const lat = props.coords.latitude;
    const long = props.coords.longitude;
    

React’s `componentWillReceiveProps` method makes it a simple matter to wait for the location data to load as well. It will run when the props are updated in the app with the coordinates of the user:

    componentWillReceiveProps(nextProps) {
        if (nextProps.coords !== null) {
            useUserLocationToMakeADifference(coords);
        }
    }
    

The [Sunlight Foundation’s Congress API](https://sunlightlabs.github.io/congress/) is a really exciting tool. Although they’ve announced recently that they are moving over management of this API to ProPublica, it is still currently hosted at their old URL and doesn’t require an API key to function. I’m not sure what the timeline is on this transition, but both parties have been quick to respond to any inquiries I’ve had about the API and are obviously doing great civic work by giving free access of this data to the public. The call I’m making to the API is very straightforward. After I have the user’s location, I simply format that into a request like this

    fetch(`https://congress.api.sunlightfoundation.com/legislators/locate?latitude=${lat}&amp;longitude=${long}`)
    

At this point it’s just a matter of formatting and presenting the response data. I pull out the relevant data from the results objects, in this case the name, phone number, and email address of each legislator. I wrote some helper methods that I won’t detail here, but the basic implementation looks a like this

    extractName(this.state.repArray[0])
    extractPhone(this.state.repArray[0])
    extractEmail(this.state.repArray[0])
    

Finally, I used an interesting trick that my mentor, Brian Douglas, showed me for rigging up an ajax spinner while the data is being fetched. When the component is mounted, the `repArray` is set to null, so I can use that state to track when the data is ready to display by using a ternary operator to render either the data or a loader.

    render () {
        return this.state.repArray ?
            <LegislatorData 1>
            <LegislatorData 2>
            <LegislatorData 3>
        : <AjaxLoader>
    }
    

### Next Steps

Midway through the development of YWFM I was fortunate enough to get a job. Between moving to Madison, WI and getting ramped up on a new technology stack, I had to put the development of several features on hold. Probably the most pressing need is to allow the user to enter their own location if they don’t want to or cannot allow the browser to access their physical location. My goal was to get the legislative contact information in front of a user in as few steps as possible, but now I need to gracefully handle that absence of the user’s location. Along the lines of “basic usability,” the app doesn’t look fantastic on mobile devices at the moment, so some work needs to be done on adjusting the styling and formatting to account for different screen sizes. I want to have the option for a user to set up reminders to call their representatives regularly. Either by having an option to populate a calendar on the device used to access the data, regular email reminders, or setting up text reminders with a service like Twilio. I think it would be interesting to be able to present recent congressional activity, whether that is votes or other recorded data. This could simply be displayed at the landing page for each legislator, or a user could similarly opt in to text or email notifications. Adding more features to the core features of the app could potentially complicate the management of state, so I am interested in adding the Redux library as a way to manage state in the app. If the idea of civic engagement through programming is interesting to you, let’s get involved! Help with any of the features above, other ideas, or even just a little help refactoring are more than welcome. [Let’s work together](https://github.com/rolfea/you-work-for-me) to make it easier for citizens to tell their legislators: “You work for me!”