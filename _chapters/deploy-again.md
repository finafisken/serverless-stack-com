---
layout: post
title: Deploy Again
date: 2017-02-14 00:00:00
code: frontend_full
description: Tutorial on how to deploy updates to your React.js single page application on AWS S3 and invalidating the CloudFront edge cache using the AWS CLI.
comments_id: 70
---

Now that we've made some changes to our app, let's deploy the updates. This is the process we are going to repeat every time we need to deploy any updates.

### Build Our App

First let's prepare our app for production by building it. Run the following in your working directory.

``` bash
$ npm run build
```

Now that are app is build and ready in the `build/` directory, let's deploy to S3.

### Upload to S3

Run the following from our working directory to upload our app to our main S3 Bucket. Make sure to replace `YOUR_S3_DEPLOY_BUCKET_NAME` with the S3 Bucket we created in the [Create an S3 bucket]({% link _chapters/create-an-s3-bucket.md %}) chapter.

``` bash
$ aws s3 sync build/ s3://YOUR_S3_DEPLOY_BUCKET_NAME
```

Our changes should be live on S3.

![App updated live on S3 screenshot]({{ site.url }}/assets/app-updated-live-on-s3.png)

Now to ensure that CloudFront is serving out the updated version of our app, let's invalidate the CloudFront cache.

### Invalidate the CloudFront Cache

CloudFront allows you to invalidate objects in the distribution by passing in the path of the object. But it also allows you to use a wildcard (`/*`) to invalidate the entire distribution in a single command. This is recommended when we are deploying a new version of our app.

To do this we'll need the **Distribution ID** of **both** of our CloudFront Distributions. You can get it by clicking on the distribution from the list of CloudFront Distributions.

![CloudFront Distributions ID screenshot]({{ site.url }}/assets/cloudfront-distribution-id.png)

Now we can use the AWS CLI to invalidate the cache of the two distributions. As of writing this, the CloudFront portion of the CLI is in preview and needs to be enabled by running the following. This only needs to be run once and not every time we deploy.

``` bash
$ aws configure set preview.cloudfront true
```

And to invalidate the cache we run the following. Make sure to replace `YOUR_CF_DISTRIBUTION_ID` and `YOUR_WWW_CF_DISTRIBUTION_ID` with the ones from above.

``` bash
$ aws cloudfront create-invalidation --distribution-id YOUR_CF_DISTRIBUTION_ID --paths "/*"
$ aws cloudfront create-invalidation --distribution-id YOUR_WWW_CF_DISTRIBUTION_ID --paths "/*"
```

This invalidates our distribution for both the www and non-www versions of our domain. If you click on the **Invalidations** tab, you should see your invalidation request being processed.

![CloudFront Invalidation in progress screenshot]({{ site.url }}/assets/cloudfront-invalidation-in-progress.png)

It can take a few minutes to complete. But once it is done, the updated version of our app should be live.

![App update live screenshot]({{ site.url }}/assets/app-update-live.png)

And that’s it! We now have a set of commands we can run to deploy our updates. Let's quickly put them together so we can do it with one command.

### Add a Deploy Command

NPM allows us to add a `deploy` command in our `package.json`.

<img class="code-marker" src="{{ site.url }}/assets/s.png" />Add the following in the `scripts` block above `eject` in the `package.json`.

``` coffee
"predeploy": "npm run build",
"deploy": "aws s3 sync build/ s3://YOUR_S3_DEPLOY_BUCKET_NAME",
"postdeploy": "aws cloudfront create-invalidation --distribution-id YOUR_CF_DISTRIBUTION_ID --paths '/*' && aws cloudfront create-invalidation --distribution-id YOUR_WWW_CF_DISTRIBUTION_ID --paths '/*'",
```

Make sure to replace `YOUR_S3_DEPLOY_BUCKET_NAME`, `YOUR_CF_DISTRIBUTION_ID`, and `YOUR_WWW_CF_DISTRIBUTION_ID` with the ones from above.

Now simply run the following command from your project root when you want to deploy your updates. It'll build your app, upload it to S3, and invalidate the CloudFront cache.

``` bash
$ npm run deploy
```

Our app is now complete. And we have an easy way to update it!
