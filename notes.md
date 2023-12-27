## AWS Accounts and Challenges

* To use _**AWS**_ you require an `AWS Account`
* AWS Account `Types` and `support plans`
    * _**Free Tier Account**_ :

        [ Refer here : https://aws.amazon.com/free/?all-free-tier.sort-by=item.additionalFields.SortRank&all-free-tier.sort-order=asc&awsf.Free%20Tier%20Types=*all&awsf.Free%20Tier%20Categories=*all ]

    * _**Support plans**_ :

        [Refer here : https://aws.amazon.com/premiumsupport/plans/ ]

        * Developer
        * Business
        * Enterprise RAMup
        * Enterprise

* When we create an AWS account for learning purposes, we have full access as we created the account ( _**root account**_ )
* Whereas in Enterprises, an AWS account is used by multiple employees and you will be one of the user of your organizational account

![Alt text](shots/3.PNG)

* To give access to the employees and set restrictions on what is allowed or denied, we need to implement _**Authentication and Authorization**_
* How organizations in `non aws/cloud world` store their user information (Identity Server) and how do we connect that to AWS (Federation)
* How to enforce Standards ?
* Service Accounts in AWS
* Generally organizations use multiple AWS accounts and in many cases we do the same _**Identity and Access Control**_ related works so how to reuse
* To Acheive the above, we need to deal with the following
    * IAM
    * Organizations and Control Tower
    * AD Sync or Federations
* Skills required:
    * JSON
    * CLI
    * Basic resources creation

## Identity and Access Management

* _**Root Account**_ : Root Account refers to the super user in AWS with access to everything including bills
* AWS can be accessed via :
    * Console Access:
        * This refers to accessing aws from https://console.aws.amazon.com/
        * Here we login with `Username` and `Password`
    * Programmatic Access:
        * This refers to accessing aws from terminal (by typing commands) and sdk (by writing code)
        * To login into aws we need _**Secret key**_ and _**Access key**_

    ![Alt text](shots/4.PNG)

* Every AWS account will have `Unique account Id`



* For everything you create in AWS will have unique _**ARN (Amazon Resource Name)**_
* Who can login into AWS? 
    * Users (_**IAM Users**_)
    * Applications
* Sometimes we might give access to an AWS Resource to access other AWS resources (_**IAM Roles**_)
* In _**AWS Authorization**_ is provided by _**IAM Policies**_
* In AWS we have two kinds of policies :
    * _**AWS Managed Policies**_ : Policies written by AWS which are available for usage in all AWS Accounts
    * _**Customer Managed Policies**_ : These are created and maintained by Customers

### Let's create a user( IAM User ) to access console

* Navigate to IAM :





* Don't give any policies



* Let's login => Navigate to console in other browser or incognito

    [Refer here : https://signin.aws.amazon.com/signin?redirect_uri=https%3A%2F%2Fconsole.aws.amazon.com%2Fconsole%2Fhome%3FhashArgs%3D%2523%26isauthcode%3Dtrue%26state%3DhashArgsFromTB_eu-north-1_6d9bb532fc2d1743&client_id=arn%3Aaws%3Asignin%3A%3A%3Aconsole%2Fcanvas&forceMobileApp=0&code_challenge=VDgSAikxnC1TX8UHJBP2iTE2GTqSgr03tIm8iuYb3TA&code_challenge_method=SHA-256 ]







* Root user has access to every thing



* The user `tony stark` doesnot have _**authorization**_
* Now let's try to give `tonystark` some permissions by `attaching policies`






### Exercise

* Create two IAM users _**(dev1, dev2)**_ and attach both of them to `AmazonEC2FullAccess Policy`
* Create two IAM users _**(test1, test2)**_ and attach both of them to `AmazonEC2ReadOnlyAccess Policy`
* Login with all the four credentails and verify the access
* Create two user groups _**developers**_ with `AmazonEC2FullAccess policy` and _**testers**_ with `AmazonEC2ReadOnlyAccess`



* This approach is useful for giving access based on user roles in your organization
* Best Practice is to give :
    * Common permissions at group level
    * Specific permissions at user level
* AWS Policies either allow or deny access

* AWS supports two types of block storage (for `virtual hard disk storage`) :
    1. _**Elastic Block Storage**_ :
        * Mounted from a different host in Same _**AZ (Availability Zone)**_
        * This is _**Persistent Storage (Non-Ephemeral)**_
        * EBS volumes can exist even after ec2 is deleted
    2. _**Instance Storage**_ :
        * Mounted from same host
        * This is _**Volatile Storage (Ephemeral)**_ 
        * Shutdown of ec2 will wipe the data adn termination (deletion) of ec2 instance will delete the disks

    ![Alt text](shots/5.PNG)
    ![Alt text](shots/6.PNG)

* EC2 charges:
    * Charge for instance type
    * Charge for EBS volumes
    * Charge for OS (for some AMI's)
* AWS EC2 mandates all the OS disks should be EBS volumes
* Only few EC2 instance types support instance storage



### AWS EC2 Lifecycle

* Instance states

![Alt text](shots/7.PNG)

* Official doc's on instance `Life cycle` :

    [Refer here : https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-lifecycle.html ]

### EC2 Instance Types

* _**Instance Family**_ : This is hardware specification of Host (T, C, M etc) and then they have generations (t1, t2, t3, c1, c5)
* _**Instance Size**_ : Generally this is a multiplication factor
    * nano
    * micro
    * small
    * medium
    * large
    * xlarge
    * 2xlarge
* Instance size also speaks about networking performance
* Purpose based Instance Families :

    [Refer here : https://aws.amazon.com/ec2/instance-types/ ]

![Alt text](shots/8.PNG)

### IAM Policies

* IAM Policy is a `JSON Document`

### JSON (Java Script Object Notation)

* We use JSON to represent data
* JSON represents data in the form of `name-value` pairs
```
name           value
qualificiation Btech
course         AWS
```
* Name-value is represented in the form of `"<name>": <value>`
```
"qualificiation": Btech
"course": AWS
```
* Value can be of different types categorized into
    1. _**Simple**_ :
        * _**Text/String**_ : we use quotes
            "qualificiation": "BTech"
            "Course": "AWS"
        * _**Number**_ :
            "duration": 90
        * _**Boolean**_ : two values are possible `true or false`
            "isOnline": true
    2. _**Complex**_ :
        * _**list/array**_ : This is represented in `[]` 
            "colors": ["Red", "White"]
        * _**object/dictionary**_ : This is represented in `{ }` 
            {
            "Street": 2,
            "Building": "Nilgiri",
            "Landmark": "Ameerpet Metro",
            "City": "Hyderabad"
            }
* _**Example-1**_ : Let's write a JSON File representing movie details :
```
{
    "Name": "RRR",
    "Director": "SS Rajamouli",
    "Cast": [
        "Ram Charan",
        "NTR",
        "Alia Bhatt"
    ],
    "Budget In Crores": 550,
    "Ratings": {
        "IMDB": 7.9,
        "RottenTomatoes": 95,
        "facebook": 5
    }
}
```
* _**Example-2**_ : Write a JSON File representing your favorite holiday destination :
```
{
    "Place": "Vishakapatnam",
    "Highlights": {
        "Spots": ["RK Beach", "Bheemili", "Araku" ],
        "Food": ["Banana Delight", "Seafood"],
        "Shopping": [] 
    }
}
```
* _**Example-3**_ : Write a JSON to represent favorite subject of yours. The structure should be as follows :
```
{
    Name = Text
    When = Text (one of School, Intermediate, Graduation, PG )
    Topics = list of text
}
```
* _**Example-4**_ : Let's write some values
```
{
    "Name": "Design and Analysis of Algorithms",
    "When": "Graduation",
    "Topics": [
        "Space Complexity",
        "Time Complexity"
    ]
}
```
* Generally we would be following structure provided by AWS for `IAM Policies` :

    [Refer here : https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_grammar.html ]

