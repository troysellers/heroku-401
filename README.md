# Heroku 401 - Pipelines

For this exercise we are going to use another Java application to work with as we setup a build pipeline in Heroku. There are a few concepts that we need to explore in the platform as to how this works. 

## Prerequisites
If you don't have a Github account, you can set one up for free now as we are going to be using this for this exercise. 

Github is the source of events that our [Heroku Pipeline](https://devcenter.heroku.com/articles/pipelines) will be listening and the source of truth for our code. It is worthwhile to take a moment and have a look at the pipelines article as it explains a lot of concpets we are about to see. 

## app.json
You might have noticed in the code repositories so far that there is this interesting file called [app.json](https://devcenter.heroku.com/articles/app-json-schema)

This file is what describes to Heroku the characteristics of the application we are going to deploy, what addons and plans are required, any configuration variables it needs and a whole host of other intersting information. It is behind an awful lot of the automation that we are using when we deploy using a pipelines and Github.

## Setup a Staging and Production Application
So you want to grab your own copy of this repository for this exercise. On this page, you should see a "fork" button, this will copy into your own Github. 

![Github Fork](images/1-githubFork.png)

We are going to do this a little differently now, let's head to the Heroku dashboard and on the top right find the "New" button and let's create a new app.

![new app](images/2-newApp.png)

Excellent, now this time we are going to give it a bit of a more meaningful name as we want it to act as our staging application. 

![staging](images/3-stagingApp.png)

Now open our app and it should be at the deploy tab. It is here that we connect our app to the codebase in our Github repository.

![app deploy](images/4-appDeploy.png)

Connect to your repository and enable automatic deploys, I usually just use the master branch but your pipeline could be different.

![auto deploy](images/5-autoDeploy.png)

Go ahead and click a manual deploy to get your code up and running on Heroku! You will see the now familiar build log as it builds and deploys.

Now go back to the home and create a new app just like before, only this time create an app with a label that you recognise as a production app. We don't need to configure anything else for this at the moment.

![app layout](images/6-appLayout.png)

## Create the Pipeline
Select your staging application and navigate to the Deploy tab again, notice the option to create a new pipeline? Click that and name it something meaningful.

![401 pipeline](images/7-401Pipe.png)

This will create the pipeline and take you to the screen where you define your flow for moving applications into production. Lets add our production app here now. 

![add production](images/8-addProd.png)

We now have an empty, shell of an application that is awaiting a deploy to production and a fully functioning application that is our staging app. 

## Enable Review Apps and Tests
So there is a very cool feature in Heroku that allows automatic (or manual) creation of a thing called a [Review App](https://devcenter.heroku.com/articles/github-integration-review-apps). A Review App is created when the platform receives a Githook from a pull request that has been created, lets have a look at what that means. 

On the left you will see a feature that says we can "Enable Review Apps", go ahead and click this button.

![enable btn](images/11-enableReviewBtn.png)

This will show us the modal where we configure how our review apps should be created, there is an interesting option that allows us to specify where we can inherit our configuration from when cretating (i.e. test databases etc.)

![enable](images/12-enableReview.png)

Finally, underneath the Tests (or Settings) tab you can go and click the button that says Enable Heroku CI and turn our pipeline into a continuous integration server as well. Neat huh :)

## Get Coding!
Now, time to grab your codebase. Remember, you want to grab the repository you have connected to your pipeline. 

```
> git clone https://github.com/ibigfoot/heroku-401.git
> cd heroku-40-1
``` 

Now we are going to build a new feature, so lets create a new local branch for us to work in. 

```
> git branch new-feature
> git checkout new-feature
```

It should look something like this when you are finished.

![new feature](images/9-newFeature.png)

Open up your favourite editor of choice (I will be using Eclipse as usual) and find our file index.ftl 

![index.ftl](images/10-header.ftl.png)

Let's make a few changes and see what happens, I am going to change the title of the page to "Building Faster with Heroku Pipelines"

![building faster](images/13-buildingFaster.png)

Now I simply commit and push this branch back to Github and create my [pull request](https://help.github.com/articles/about-pull-requests/).

```
> git commit -am "new feature done"
> git push origin new-feature
```

![git push](images/14-gitPush.png)

![pull request](images/15-pullRequest.png)

Make ready with the Heroku Pipeline open as well, if you have enabled automatic deploys you can see this happen if you are quick.

## Managing a Release
Back into my pipeline the Release Manager can see that there is a new Review App that has been created and is ready for inspection, along with a link directly to the pull request.

If you are quick enough, you will see the Review App getting ready for an automatic deploy

![review deploy](16-reviewAppDeploy.png)

The build manager now looks at this change live, checks the documentation in the pull request and merges the change into the master (or staging) branch. Now the automatic deploy also fires to bring the staging app back up with the latest version as well. 

If you enabled the CI feature, you should be able to see this in your tests section

![Testing](images/17-autoTest.png)

The deploy will wait until test have run and passed, but once they do you now have a fully functional app that is linked to the pull reqeust that created it. Go ahead and open your app and check your change is functional.

![review deployed](images/18-reviewDeployed.png)

Looks good, go ahead and merge the pull request. This should also start another build on your staging app as it automatically deploys the lastest version of code. Once the testing has finished you should be able to view the build log from the staging pipeline.

![build log](images/19-buildLog.png)

Lastly, let's promote into production 

![promote](images/20-promoteStaging.png)
![promote](images/21-promoteStaging.png)
![promote](images/22-promoteStaging.png)



