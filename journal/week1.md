# Week 1 — App Containerization
## Containerized cruddur app
# Required
To Containerize crudder app. Crudder app was wriiten in react js for frontend and python for backend. I followed the instructions in the videos and then did the home work challenges and merged the two branches together. I also pushed my image to docker hub. <b>
Here's how I containerized the app using best practice, multi-staging and also ran dockerfile CMD as an external script, configured database, commit and pushed to github, then to dockerhub.

STEP 1:
I cloned the app repository for cruddur and opened it with gitpod workspace.
I reviewed the frontend and backend code then set to write a dockerfile

STEP 2:
After watching the live video on writing a dockerfile, I wrote one and built the image. I decided to add the homework challange by writing the dockerfile in a multi-staging format to write the dockerfile, see code below <br>
Multi-stage builds are a feature of Docker that allow you to create smaller and more efficient Docker images by dividing the build process into multiple stages.
The first stage (build) installs dependencies, builds the application, and produces the compiled artifacts. The second stage copies only the compiled artifacts from the first stage, and sets up the runtime environment to run the application.
  <br>
## Dockerfile for Frontend
```
# using multi-staging to build the app
# Stage 1: Build the application
FROM node:16.18 AS build

WORKDIR /frontend-react-js

COPY . /frontend-react-js

RUN npm install
RUN npm run build

# Stage 2: Copy the built application into a production-ready image
FROM node:16.18-slim

ENV PORT=3000

WORKDIR /frontend-react-js

COPY --from=build /frontend-react-js/build /frontend-react-js
COPY --from=build /frontend-react-js/package*.json /frontend-react-js/

RUN npm install

EXPOSE ${PORT}
COPY start.sh .
# Run the dockerfile CMD as an external script
CMD ["./start.sh"]
```

<b>
I created 'start.sh' file, a bashscript to start the app, see below the content of the bashscript
  
```
#!/bin/bash
npm start
```
<br>
Step 3: Ran `npm i` in the frontend folder to install dependencies for the frontend app
After installing I wrote a Dockerfile for the <b>backend</b>, see code below:
  <br>
Step 4: Wrote Dockerfile for Backend
<br>

```
# using multi-staging to build the app

# Stage 1: Build the application
FROM python:3.10-slim-buster AS builder

WORKDIR /backend-flask

COPY requirements.txt requirements.txt
RUN pip3 install --user -r requirements.txt

COPY . .

# Stage 2: Copy the application into a production-ready image
FROM python:3.10-slim-buster

WORKDIR /backend-flask

COPY --from=builder /root/.local /root/.local
COPY --from=builder /backend-flask .

ENV PATH=/root/.local/bin:$PATH
ENV FLASK_ENV=development

EXPOSE ${PORT}

COPY entrypoint.sh .

# Run the dockerfile CMD as an external script
CMD ["./entrypoint.sh"]
```

<br>
The entrypoint.sh file was created in the backend folder. See below the content of the file.
<br>

```
#!/bin/bash
python3 -m flask run --host=0.0.0.0 --port=4567
```

<br>
This script can be used to quickly start a Flask server on a specified port and make it accessible from any IP address. Here, it is listening to incoming request via port 4567.
  <br>

Step 5: Write a Docker compose file to run both frontend and backend at the same time.
See code below:
```
version: "3.8"
services:
  backend-flask:
    environment:
      FRONTEND_URL: "https://3000-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
      BACKEND_URL: "https://4567-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
    build: ./backend-flask
    ports:
      - "4567:4567"
    volumes:
      - ./backend-flask:/backend-flask
  frontend-react-js:
    environment:
      REACT_APP_BACKEND_URL: "https://4567-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
    build: ./frontend-react-js
    ports:
      - "3000:3000"
    volumes:
      - ./frontend-react-js:/frontend-react-js
networks: 
  internal-network:
    driver: bridge
    name: cruddur

volumes:
  db:
    driver: local
```

Step 6: Built the App using the command docker-compose up and opened the ports so that the app can be accessed.
After using the docker-compose up command to build the app, I was able to view the app and login using my email and password:1234
  
Step 7: Created notification for the frontend and backend. See steps below
  <b>For Backend</b>
  (i) cd into backend-flask folder, edited the openapi.yml file in the folder by adding a block of code for notifications. see code below:
 <br>
  
  ```
  api/activities/notifications:
    get:
      description: 'Return the followers i followed'
      tags:
        - activities
      responses:
        '200':
          description: Returns an array of activities"
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Activity'
  ```
 
  
<br>
  
  (ii) cd into app.py, added the following code, one to the import block part of the code and the other to the body. See code below;

<br>
  
  ```
from services.notifications_activities import *
  
@app.route("/api/activities/notifications", methods=['GET'])
def data_notifications():
  data = NotificationsActivities.run()
  return data, 200
   ```
 
  <br> 
 There were both added just below the home section according to the ui of the app.
  <br>
   (iii) Lastly, cd into /backend-flask/services. Created a file named notifications_activities.py and added this code below. template was gotten from the home.py
  <br>
  
```
from datetime import datetime, timedelta, timezone
class NotificationsActivities:
  def run():
    now = datetime.now(timezone.utc).astimezone()
    results = [{
      'uuid': '68f126b0-1ceb-4a33-88be-d90fa7109eee',
      'handle':  'Gladys Godwin',
      'message': 'Cloud is fun but it takes hardwork and constitency!',
      'created_at': (now - timedelta(days=2)).isoformat(),
      'expires_at': (now + timedelta(days=5)).isoformat(),
      'likes_count': 5,
      'replies_count': 1,
      'reposts_count': 0,
      'replies': [{
        'uuid': '26e12864-1c26-5c3a-9658-97a10f8fea67',
        'reply_to_activity_uuid': '68f126b0-1ceb-4a33-88be-d90fa7109eee',
        'handle':  'Choja',
        'message': 'I miss you Gladys, hope cloud is treating you well!',
        'likes_count': 0,
        'replies_count': 0,
        'reposts_count': 0,
        'created_at': (now - timedelta(days=2)).isoformat()
      }],
    }
    ]
    return results
  ```
  
  <br>
  This created a backend api for notifications, hence i could view my backend endpoint using the app url/api/activities/notifications as seen in the photo below
  <br>
  
  ![backendapi](https://user-images.githubusercontent.com/99274632/221462789-68466dd9-ce9f-4374-a62b-2f2ae30d9a6b.PNG)
  
  <br>
  <b>For Frontend</b>
 (i) cd into frontend-react-js folder, edited the App.js file, adding the notification block to the code in the app.js file just below the home. 
<br>

```  
import NotificationsFeedPage from './pages/NotificationsFeedPage';
  
 {
    path: "/notifications",
    element: <NotificationsFeedPage />
  },
```

  <br>
  (ii) cd into <b>src</b>
  <br>
  (iii) created two files; NotificationFeedPage.js and NotificationFeedPage.css. Added the following code(copied from the HomeFeed.js but edited home to notifications)
  <br>
  
  ```
import './NotificationsFeedPage.css';
import React from "react";

import DesktopNavigation  from '../components/DesktopNavigation';
import DesktopSidebar     from '../components/DesktopSidebar';
import ActivityFeed from '../components/ActivityFeed';
import ActivityForm from '../components/ActivityForm';
import ReplyForm from '../components/ReplyForm';

// [TODO] Authenication
import Cookies from 'js-cookie'

export default function NotificationsFeedPage() {
  const [activities, setActivities] = React.useState([]);
  const [popped, setPopped] = React.useState(false);
  const [poppedReply, setPoppedReply] = React.useState(false);
  const [replyActivity, setReplyActivity] = React.useState({});
  const [user, setUser] = React.useState(null);
  const dataFetchedRef = React.useRef(false);

  const loadData = async () => {
    try {
      const backend_url = `${process.env.REACT_APP_BACKEND_URL}/api/activities/notifications`
      const res = await fetch(backend_url, {
        method: "GET"
      });
      let resJson = await res.json();
      if (res.status === 200) {
        setActivities(resJson)
      } else {
        console.log(res)
      }
    } catch (err) {
      console.log(err);
    }
  };

  const checkAuth = async () => {
    console.log('checkAuth')
    // [TODO] Authenication
    if (Cookies.get('user.logged_in')) {
      setUser({
        display_name: Cookies.get('user.name'),
        handle: Cookies.get('user.username')
      })
    }
  };

  React.useEffect(()=>{
    //prevents double call
    if (dataFetchedRef.current) return;
    dataFetchedRef.current = true;

    loadData();
    checkAuth();
  }, [])

  return (
    <article>
      <DesktopNavigation user={user} active={'notifications'} setPopped={setPopped} />
      <div className='content'>
        <ActivityForm  
          popped={popped}
          setPopped={setPopped} 
          setActivities={setActivities} 
        />
        <ReplyForm 
          activity={replyActivity} 
          popped={poppedReply} 
          setPopped={setPoppedReply} 
          setActivities={setActivities} 
          activities={activities} 
        />
        <ActivityFeed 
          title="Notifications" 
          setReplyActivity={setReplyActivity} 
          setPopped={setPoppedReply} 
          activities={activities} 
        />
      </div>
      <DesktopSidebar user={user} />
    </article>
  );
}
```

<br>
  <b>Content of NotificationFeed.css</b> 
<br>

```
  article {
    display: flex;
    flex-direction: row;
    justify-content: center;
  }
```

<br>
  
<b>After adding these codes I will able to test the frontend endpoint,see result as follows</b>
<br>
  
![frontendapi](https://user-images.githubusercontent.com/99274632/221464353-7b72b87e-b234-4042-a2b2-229f1b525308.PNG)
