🛠️ What Is AWS CodeBuild?
CodeBuild is a fully managed continuous integration service that compiles source code, runs tests, and produces deployable artifacts. It scales automatically and eliminates the need to manage your own build servers.

![alt text](image-5.png)

Step 1: S3 bucket:

1. Go to s3 bucket > create bucket
    Give name
![alt text](image-6.png)

2. Uncheck block public access -- toaccess bucket content/site from internet
![alt text](image-7.png)

3. Create Bucket

4. On S3 buckects page select our bucket > Permission
![alt text](image-8.png)

Policy: 
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::YOUR-BUCKET-NAME/*"
            ]
        }
    ]
}

Where: 
"Principal": "*" → Allows everyone (public access)

"Action": "s3:GetObject" → Grants permission to download/view files

"Resource" → Targets all objects (*) in the specified bucket


5. Enable Static Hosting for Angular App 
    * Goto Properties tab > edit Static hosting > enable
    * Set index doc and error doc as index.html (for SPA)
    * Save
![alt text](image-9.png)


STEP2: Creating Code pipe line with Code build

1. Create Pipenline
2. Choose Custom build
   ![alt text](image-10.png)
3. In choose pipeline settings 
    * give name
    * rest keep every thing default
   ![alt text](image-12.png)
4. Select Source as Github app > select github connection > repo > branch
![alt text](image-13.png)
5. Add Build Stage
![alt text](image-14.png)
    select other build provider > aws code buold > Create Project

6. Create Code build Project > give name
    * Open addition settings 
    * Check on "Restrict number of concurrent builds this project can start" --- this will prevent concurrent builds and cost 
    * make build limit as 1
    * ![alt text](image-15.png)

7. Select opearing system , image based on the runtime env like node version
![alt text](image-16.png)

8. Select BuildSpec file
![alt text](image-17.png)

9. rest can be default settings > continue to code pipepline
![alt text](image-18.png)

10. Runtime env [text](https://docs.aws.amazon.com/codebuild/latest/userguide/available-runtimes.html)

11. Select Deployment provider (S3 Bucket in this case)
![alt text](image-19.png)

12. Select checkbox extract before deployment
![alt text](image-20.png)

13. buildspec:
version: 0.2  # Specifies the BuildSpec schema version—must be 0.2 for this structure

phases:
  install:  # 🧰 Phase 1: Install essential tools and define environment
    runtime-versions:
      nodejs: 20  # ⚙️ Set the Node.js runtime to v20 for building the project
    commands:
      - npm install -g @angular/cli@17  # 🚀 Install Angular CLI globally, version 17

  pre_build:  # 🔧 Phase 2: Prepare project before actual build starts
    commands:
      - npm install  # 📦 Install all dependencies from package.json using npm

  build:  # 🏗️ Phase 3: Compile and package your app
    commands:
      - ng build -c production  # 🔨 Use Angular CLI to build the app in production mode

artifacts:
  base-directory: dist/my-angular-project  # 📁 Folder where the Angular build outputs are stored
  files:
    - '**/*'  # 📦 Include all files recursively within base-directory for deployment or download
 
 14: 