[//]: README.md

# Deploy a Static Website to AWS Amplify
## from a BitBucket Repository using a Domain Name registered by GoDaddy  
### *Edwin W. Meyer*
### December 31, 2023

## Background
Described here is how to create a static web application hosted by Amazon Web Services using its AWS Amplify service, 
with code deployed from a BitBucket repository. The required procedure involves Git, BitBucket, AWS Amplify, 
AWS Route 53, and GoDaddy. Following is a summary of the required steps:  
1. Set up a local Git repo for your project and place your content into it.   
2. Push your repo to BitBucket.
3. Create an AWS account (if necessary).
4. In AWS Amplify: Deploy the code for your static website from BitBucket to AWS Amplify.
5. In AWS Route 53: Create AWS Hosted Zone and copy the name server URLs.
6. In GoDaddy: Set AWS Nameservers into GoDaddy DNS Management.
7. In AWS Amplify: Configure Root Domain.
8. Test Your Application Running in AWS.

Note: One can also deploy to AWS Amplify from other repo services, including Bitbucket, GitLab, or AWS CodeCommit. 
Or the code can be directly uploaded from a local repository. Also, while this tutorial describes using a 
domain name registered with GoDaddy, other registrars can be used including AWS Route 53.
In either case, some modifications will be required to the directions.

## Documentation Conventions
Except where specially noted:
- "\-" at the beginning of a line denotes instructions to be executed.
- "$" at the beginning of a line represents the terminal command prompt.
- ">" at the beginning of a line denotes a URL to be entered into a web browser.
- "-->" at the end of a terminal command indicates that the following text on the same or subsequent lines is the expected output.
- Italicized text always is a comment.

## Set up local Git repo
*Git must be installed. See ```https://git-scm.com/book/en/v2/Getting-Started-Installing-Git```*  
$ cd ~/projects/homepage  # This is the project root for this tutorial  
$ git init -->  Using 'master' as the name for the initial branch.   

## Perform Initial Commit
- (optional) Create a .gitignore file in the project root containing any files or directories that are not to be 
$ git add . # add all unignored files.  
$ git commit -m "Initial commit"

## Create the Repo on Bitbucket
&nbsp;&nbsp;&nbsp;&gt; bitbucket.org  
- Click the user's image towards the top-right of the window and under "Recent workspaces" or within "All workspaces".   
- Select the logged-in user. # Necessary only if the user has access to several workspaces.  
- Select the Create tab --> The "Create a new repository" page.  
- Select "Untitled project" as Project, "homepage" as Repository name, leave Access level as "Private repository", 
  Include a README? : No, Default branch name: master, Include .gitignore?: No  
- Click "Create repository" --> "Let's put some bits in your bucket" page  

## Push Local Repo to Bitbucket
*per the customized instructions under the "Get your local Git repository on Bitbucket" section:*  
$ cd ~/projects/homepage  
$ git remote add origin git@bitbucket.org:edwinmeyer/homepage.git  
$ git push -u origin master --> 
"* [new branch]  master -> master"    
*The '-u' option causes the local 'master' branch to be set up to track the remote branch 'master' from 'origin'.*

## AWS Account Signup
*Create an AWS account if you don't already have one.*
-  Delete all aws.amazon.com cookies. *Optional but useful if you have previously worked with AWS*  
  &gt; https://portal.aws.amazon.com/billing/signup --> "Sign in" page 
- Leave "Root user" selected  
- Click "Create a new AWS account" --> "Sign up for AWS" page
- Enter:
  Root user email address: someone@example.com *Substitute your own email address*
  AWS account name: static-web-hosting *Or a name of your choosing*
- Click "Verify email address" -> "Confirm you are you" page
- Enter emailed 5-digit verification code & click "Verify" --> "Create your password" page
- Enter & Confirm a Root user password
- Type the obscured "Security check" characters
- Click "Continue (step 1 of 5)" --> "Contact Information" page
- Select the "Personal" use radio button (or Business)
- Enter Full Name, (cell) Phone Number, Country or Region, (1 or 2 line) Address, City, State, Postal Code, and agree 
  to the terms of service
- Click "Continue (step 2 of 5)" --> "Billing Information" page
- Enter credit or debit card info + the obscured "Security check" text.
- Click "Verify and Continue (step 3 of 5)" --> "Confirm your identity" page
- Enter 4-digit code texted to cell phone & click continue --> "Select a support plan" page
- Keep "Basic support - Free" radio button checked & click "Complete sign up" --> "Congratulations!" page
- Click the "Go to the AWS Management Console" button --> "Sign in" page

## Deploy Static Website from BitBucket to AWS Amplify
&nbsp;&nbsp;&nbsp;&gt; https://aws.amazon.com/
- Click "Sign in to the Console" --> "Sign-in" page
- Enter your email as the "Root user email address", then click Next --> "Root user sign in" page  
- Enter your password and click "Sign in" --> AWS Console Home
- If "AWS Amplify" appears in the "Recently visited" section, click AWS Amplify.  
  Otherwise, click "View all services" 
  at the bottom of the section, then click "AWS Amplify" in the "Front-end Web & Mobile" section  
  --> "All apps" page
  (in both cases)  
- Click "New app" towards the top right side of the page then "Host web app" in the dropdown --> "Get Started under 
  Amplify Hosting."
- In the "From your existing code" section, click the Bitbucket radio button, then click Continue --> 
  Confirm access to your account"
- Click "Grant Access" --> "Add repository branch" with "Bitbucket authorization was successful." legend.
- Under "Recently updated repositories", select the repository and specify the branch as "master". Then click "Next"  
  --> "Build settings" page
- Check "Allow AWS Amplify to automatically deploy all files hosted in your project root directory" & click Next --> Review page
- Click "Save and deploy" --> "homepage" with AWS Amplify App Settings in the left-hand column.
- In the "App settings | Rewrites and redirects" page, click Edit  
- Change the following in the single "Rewrites and redirects" entry:  
  Source address: "/" *the website root -- so only the root URL is redirected*
  Target address: "index.htm" *Change from "index.html" to whatever your index page is named*  
  Type: "200 (Rewrite)" *The browser will continue to display the URL the user entered*  
- Click Save  Create AWS Hosted Zone & Copy Name Servers
*A URL similar to *https://master.di3q3s4tuy4em.amplifyapp.com/* is presented*
- Click the URL *to confirm that your website is displayed*

## Make AWS Route 53 the DNS service
### Create AWS Hosted Zone and copy the name server URLs.
*per https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/migrate-dns-domain-inactive.html* and  
*https://dev.to/dabit3/how-to-deploy-a-custom-domain-name-with-the-amplify-console-in-under-two-minutes-jhh*  
&nbsp;&nbsp;&nbsp;&gt; https://aws.amazon.com/ & click "Sign In to the Console" --> Sign in page
- Sign in if necessary
- Enter "Route 53" into the search area at theGoDaddy  top  of the page --> "Amazon Route 53" page
- Click "Get started", select the "Create hosted zones" radio button and click "Get started" on this page --> "Create 
  hosted zone" page
- Enter "example.com" (or your own domain) as the "Domain name", an optional description, and leave the Type as "Public 
  hosted zone". Then click "Create hosted zone" --> Route 53 | Hosted zones | example.com
- Open the "Hosted zone details" section and copy the four entries in the "Name servers" subsection to the clipboard. 
  They should look similar to the following:
  ns-1953.awsdns-45.co.uk
  ns-18.awsdns-01.com
  ns-987.awsdns-58.net
  ns-1440.awsdns-54.org
  
### In GoDaddy: Set AWS Nameservers into GoDaddy DNS Management
&nbsp;&nbsp;&nbsp;&gt; https://dcc.godaddy.com/manage/example.com/dns * Substitute your own domain name for "/example.com"
--> "DNS Management" page in GoDaddy
- Select the Nameservers tab & note the two default nameservers for backup, e.g.: {ns48, ns49}.domaincontrol.com  
  *These will be changed to the Route 53 nameserver domains*
- Click "Change Nameservers", select "I'll use my own nameservers" in the popup, and enter the above name servers copied 
  from AWS Route 53. Then click Save --> "Edit nameservers" popup with a "risky" legend.
- Click Continue. --> back to the DNS Management page which briefly displays "Success".  
- Refresh the page to see Route 53 nameservers.

### In AWS Amplify: Configure Root Domain
&nbsp;&nbsp;&nbsp;&gt; https://us-east-1.console.aws.amazon.com/amplify/home & click "homepage"
- Click on "Domain management" under "App Settings" in left sidebar --> Domain management page
- Click "Add domain", then enter root domain (example.com) and click "Configure domain"
- Ignore the Subdomains section & click Save --> Domain management status page  
*Wait until "Domain activation" is finished.*
  
### Test Your Application Running in AWS
- Now attempt to access your static web app from a web browser, e.g.
&nbsp;&nbsp;&nbsp;&gt; example.com *or your own domain*
  
Very hopefully your web page running on AWS Amplify is displayed. If not, enter the AWS Amplify native URL, which should 
still work, e.g.  
&nbsp;&nbsp;&nbsp;&gt; https://master.di3q3s4tuy4em.amplifyapp.com/  
If this doesn't display your website, try repeating the initial steps. If the AWS Amplify native URL displays you 
webpage, your problem is with the domain name forwarding you set up.  
If entering the native AWS Amplify URL works, it is quite possible that the propagation delay that AWS setup introduces 
to various internal servers is responsible.

You might also want to try the following URLs (where example.com is replaced with your own domain):  
&nbsp;&nbsp;&nbsp;&gt; http://www.example.com  
&nbsp;&nbsp;&nbsp;&gt; https://example.com  
&nbsp;&nbsp;&nbsp;&gt; http://example.com  

If that doesn't work, the only thing I can suggest is to repeat your work, carefully inspecting the results of each step. 
Be aware that this issue may not be where you think it is. If it's any consolation, I tried these steps for hours until I finally found my errors.

This README text is copyright &copy; 2023-2024 Edwin Meyer Software Engineering.
However it may be reproduced in whole or in part if the copyright legend is included.
