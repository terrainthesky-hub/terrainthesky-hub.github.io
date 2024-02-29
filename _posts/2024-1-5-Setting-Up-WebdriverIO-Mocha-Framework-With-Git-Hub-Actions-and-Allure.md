---
layout: post
title: Setting up WebdriverIO Mocha Framework With Docker, GitHub Actions and Allure.
subtitle: WebdriverIO, npm, Typescript, Mocha, Docker, Github Actions, Allure
---


There's quite a few different technologies at work, but we're going to assume you have and can use, Git, VS Code, Git Bash. We'll begin starting off with setting up WebdriverIO, Mocha, and Allure.

In your main directory, in the bash terminal, enter this to begin the instillation of WebdriverIO and Mocha for the WebdriverIO Command Line Interface.
        
        npm install @wdio/cli

After, begin the configuration with

        npx wdio config

These are the following options to choose:

![image](https://github.com/terrainthesky-hub/terrainthesky-hub.github.io/assets/60892621/f9bee03f-cda5-429b-9e26-3eb4fb4486cb)

![image](https://github.com/terrainthesky-hub/terrainthesky-hub.github.io/assets/60892621/74a6a37c-ae4a-4405-82b4-4bf314da960e)

![image](https://github.com/terrainthesky-hub/terrainthesky-hub.github.io/assets/60892621/8b9eebcf-a0ab-4b36-91fc-a658be7eacc6)

![image](https://github.com/terrainthesky-hub/terrainthesky-hub.github.io/assets/60892621/3d098dcf-6a2a-4beb-aeb7-1f975d8df201)

  Press space to select:

![image](https://github.com/terrainthesky-hub/terrainthesky-hub.github.io/assets/60892621/b2e68906-5679-4102-b1ef-caac80644a06)

![image](https://github.com/terrainthesky-hub/terrainthesky-hub.github.io/assets/60892621/8c250dee-58cc-455b-b0da-8fd67c1d6157)

![image](https://github.com/terrainthesky-hub/terrainthesky-hub.github.io/assets/60892621/b18c700b-fe9e-489e-b18e-8ba136f5bc6f)

![image](https://github.com/terrainthesky-hub/terrainthesky-hub.github.io/assets/60892621/c04a1aff-d2d4-4020-9595-70fcb6d2ca62)

![image](https://github.com/terrainthesky-hub/terrainthesky-hub.github.io/assets/60892621/b2bc33a9-8a59-4855-89b1-bceac0155d38)

![image](https://github.com/terrainthesky-hub/terrainthesky-hub.github.io/assets/60892621/d7b0f5d3-873f-4a85-b05f-5688c22e63c7)

![image](https://github.com/terrainthesky-hub/terrainthesky-hub.github.io/assets/60892621/a7d06830-a53d-42e3-8696-ee7f556751bf)

  Choose more here if you need them:

![image](https://github.com/terrainthesky-hub/terrainthesky-hub.github.io/assets/60892621/a5ef8b00-02fb-45c1-a08f-8a461f560357)

![image](https://github.com/terrainthesky-hub/terrainthesky-hub.github.io/assets/60892621/ead8d4d5-655a-44eb-90ac-7fd9caef2449)

![image](https://github.com/terrainthesky-hub/terrainthesky-hub.github.io/assets/60892621/ba955ac4-7097-4acf-811f-a0173d8eaa10)

![image](https://github.com/terrainthesky-hub/terrainthesky-hub.github.io/assets/60892621/3e104f0e-f7cb-4755-ad7b-53d4ae4e6785)

![image](https://github.com/terrainthesky-hub/terrainthesky-hub.github.io/assets/60892621/1c8744ab-716e-4723-afcf-88afc5c27653)

![image](https://github.com/terrainthesky-hub/terrainthesky-hub.github.io/assets/60892621/1283a63e-c627-4319-b14c-cdd2a23cb42b)

From here you can begin to add tests to your spec files or just use the default ones made in the configuration. Test out the default tests by typing in
                        
        npm run wdio
                        
Next, you want to configure your wdio.conf.ts file correctly to work with the runners and services like Docker and Allure. I'm using the default port for docker for now:
        
        services: [
                ['docker', {
                    // Docker options
                    image: 'selenium/standalone-chrome',
                    healthCheck: 'http://localhost:4444',
                    options: {
                        p: ['4444:4444'],
                        // Add other Docker options here
                    },
                    // Add other wdio-docker-service configurations here
                }],
            ],

Configure your reporter to this:

        reporters: [
                'spec',
                ['allure', {
                  outputDir: 'allure-results',
                  disableWebdriverStepsReporting: true,
                  disableWebdriverScreenshotsReporting: false,
                }],
                ],

Next, you'll want to get your package.json file set up for using Allure, Mocha, and Chrome:

        {
          "name": "my-new-project",
          "type": "module",
          "devDependencies": {
            "@wdio/allure-reporter": "^8.27.0",
            "@wdio/cli": "^8.26.1",
            "@wdio/local-runner": "^8.26.1",
            "@wdio/mocha-framework": "^8.24.12",
            "@wdio/spec-reporter": "^8.24.2",
            "allure-commandline": "^2.25.0",
            "allure-mocha": "^2.10.0",
            "chromedriver": "^119.0.1",
            "ts-node": "^10.9.1",
            "typescript": "^5.3.3",
            "wdio-chromedriver-service": "^8.1.1",
            "wdio-docker-service": "^3.2.1",
            "wdio-wait-for": "^3.0.9"
          },
          "scripts": {
            "wdio": "wdio run ./wdio.conf.ts",
            "allure:report": "allure generate allure-results --clean"
          },
          "dependencies": {
            "allure": "^0.0.0",
            "string-width": "^6.1.0"
          },
          "mocha": {
            "parallel": false,
            "reporter": "allure-mocha",
            "reporterOptions": {
              "resultsDir": "allure-results"
            }
          }
        }

Now you should have Docker installed, and create a Dockerfile with this configuration. Use environment variables to open the port you want:

        # Use an official Node runtime as a parent image
        FROM node:latest
        
        # Install Chrome for running tests
        RUN apt-get update && apt-get install -y wget gnupg2
        RUN wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add -
        RUN sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list'
        RUN apt-get update && apt-get install -y google-chrome-stable
        
        # Install other dependencies
        RUN apt-get install -y libnss3 libgconf-2-4 libfontconfig1
        
        # Set the working directory in the container
        WORKDIR /usr/src/app
        
        # Copy the current directory contents into the container at /usr/src/app
        COPY . .

        # Expose the port dynamically
        EXPOSE $PORT
        
        # Install any needed packages specified in package.json
        COPY package*.json ./
        RUN npm install
        
        # Run WebdriverIO tests when the container launches
        CMD ["npx", "wdio", "wdio.conf.ts"]

Next, you'll create a folder called .github which contains a folder called workflows, then create a file called main.yml. Here is the configuration to install ubunutu, build a Docker image, run the Docker image, and share the allure-results folder with your Github Actions CLI. This will use a default security token from Github, and create an artifact that it shares between the Docker Image and the Github Actions CLI:

        name: CI

        on:
          push:
            branches: [ main ]
          pull_request:
            branches: [ main ]
        
        jobs:
          build-and-run:
            runs-on: ubuntu-latest
        
            steps:
            - uses: actions/checkout@v2
        
            - name: Build the Docker image
              run: docker build . --file Dockerfile --tag your-docker-image-name-here
        
            - name: Run the Docker image
              run: docker run -v ${PWD}/allure-results:/usr/src/app/allure-results -p 4444:4444 your-docker-image-name-here || true
        
            - name: Upload Allure results
              uses: actions/upload-artifact@v2
              with:
                name: allure-results
                path: ./allure-results
          
            - name: Download Allure results
              uses: actions/download-artifact@v2
              with:
                  name: allure-results
        
            - name: Install Allure CLI
              run: npm install -g allure-commandline
        
            - name: Generate Allure Report
              run: npm run allure:report
        
            - name: Deploy to GitHub Pages
              uses: peaceiris/actions-gh-pages@v3
              with:
                github_token: ${{ secrets.GITHUB_TOKEN }}
                publish_dir: ./allure-report
                publish_branch: gh-pages

To configure github, you'll want to go to the settings and pages, and make sure you have this as set up for the default Github Pages:

![image](https://github.com/terrainthesky-hub/terrainthesky-hub.github.io/assets/60892621/9ef54d97-562c-4dc5-a0bf-956a8deb1992)

Go to settings, Actions, then General and set your permissions to this:

![image](https://github.com/terrainthesky-hub/terrainthesky-hub.github.io/assets/60892621/a96f17c2-4fb3-4861-bacc-5e750f946c86)

![image](https://github.com/terrainthesky-hub/terrainthesky-hub.github.io/assets/60892621/98454d77-7c7c-4f3e-b6ac-8332e5cc6da0)


Now in your bash terminal do "git add .", then git commit -m "your commit name", and git push to see it run your Docker container, test suite, and create your allure report on the gh-pages branch hosted by github pages. I have a purposefully added a failed test to show it will run even if one of the tests fails:

![image](https://github.com/terrainthesky-hub/terrainthesky-hub.github.io/assets/60892621/624f4825-042c-4a3b-9419-6b10626bf4d2)

Now you can configure Allure, the framework, and other github permissions as needed.
