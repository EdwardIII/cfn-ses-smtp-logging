# Write SES Delivery Notifications to CloudWatch 

If you're used to running any kind of mail service, you will
know there are times when you have to figure out whether
an email was delivered somewhere or not. Typically when recipients say
they aren't getting emails and you need to identify if the problem is their end or yours.

## CloudFormation template
This CloudForomation sets that up for you:

1. Create a Lambda to accept SES notifications

2. Allow that Lambda to write to CloudFormation

## Manual steps

The template doesn't include these bits, so you'll need to do it manually:

1. Go to `SES > Configuration Sets`

2. Create a new `SNS Destination`

3. Tick the `Event types` you want to receive (this setup hasn't been tested with Click and Open)

4. Choose a name like `SesSmtpLogging` (you will need to use this in your email sending app)

5. Choose the topic created previously by CloudFormation, `sns-smtp-log`

5. Save

Next up when sending your SES email via the API you will need to tell it to use this `ConfigSet`:


```js
{
  Destination: {
    ToAddresses: ["destination@host.com"]
  },
  Source: "me@my-website.com",
  ConfigurationSetName: 'SmtpLogging' // unless you used a different name than that above
  // etc...
}
```

## All Done

Send some test emails and you should now see results in appearing in 
cloudwatch under something like: 

`sns-smtp-logging-SnsToCloudwatch-XXXX`

See the links below for an example of all the events SES will
send.



# Useful articles

[Examples of event data that Amazon SES publishes to Amazon SNS](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/event-publishing-retrieving-sns-examples.html#event-publishing-retrieving-sns-delivery)

[Setting up event publishing](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/monitor-using-event-publishing.html)

