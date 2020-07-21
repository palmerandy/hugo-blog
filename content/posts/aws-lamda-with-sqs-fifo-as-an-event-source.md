---
title: "AWS Lamda with SQS FIFO as an event source"
date: 2020-07-14T20:21:00+10:00
draft: false
tags: ["AWS", "AWS SQS", "AWS Lambda"]
---

In late 2019 [AWS announced support](https://aws.amazon.com/blogs/compute/new-for-aws-lambda-sqs-fifo-as-an-event-source/) for SQS FIFO queues as an event source for AWS Lambdas.  

### Amazon SQS FIFO (First-In-First-Out) queues
Amazon's Simple Queue Service supports [FIFO as per the docs](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/FIFO-queues.html):

> FIFO (First-In-First-Out) queues are designed to enhance messaging between applications when the order of operations and events is critical, or where duplicates can't be tolerated

### Message Grouping
One of their amazing features SQS FIFO queues is the ability to have sequenced message that are grouped.  A great use case of this might be tracking multiple race distances that occur at a fun run, i.e. as each participant finishes in their race distance you want to capture the order they finished for the group (that is their race).  In this way all the 5 km race participants are captured in order, and so on for each race distance (e.g. 10 km, half marathon, marathon).   

The [AWS docs summarize](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/FIFO-queues.html) this as:

> Messages that belong to the same message group are always processed one by one, in a strict order relative to the message group (however, messages that belong to different message groups might be processed out of order).

You might be thinking what happens if a Lambda that has the queue as an event source encounters an error?  Well take the above scenario about multiple fun runs happening concurrently, so for simplicity sake imagine two message groups existing one for the 5 km race and another for the 10 km race.  The same lambda would be consuming both groups.  If the 10 km message group encountered an error at 5th place, the subsequent messages for 10 km would be blocked until the 5h place message was successfully processed.  Importantly the other races such the the 5 km race would continue to process.  