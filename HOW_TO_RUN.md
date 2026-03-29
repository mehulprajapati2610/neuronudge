# How to Run NeuroNudge

## The ONLY correct way to access the app

1. Start the Spring Boot backend in IntelliJ
2. Open your browser and go to: **http://localhost:8080/login.html**

DO NOT open HTML files by double-clicking in file manager.
The file:// protocol cannot make API calls to the backend.

---

## Step-by-step setup

### 1. Configure application.properties
Open: `backend/src/main/resources/application.properties`

Replace these values with yours:
```properties
# Your MongoDB Atlas URI (include database name at the end)
spring.data.mongodb.uri=mongodb+srv://username:password@cluster.mongodb.net/neuronudge

# Your Gemini API key (get from https://aistudio.google.com/app/apikey)
gemini.api.key=AIzaSy...your_key_here

# Email (optional - for burnout alerts)
spring.mail.username=your_gmail@gmail.com
spring.mail.password=your_16_char_app_password
app.email.enabled=true
```

### 2. Run in IntelliJ
- Open the `backend` folder as a Maven project in IntelliJ
- Run `NeuroNudgeApplication.java`
- Wait for "Started NeuroNudgeApplication" in the console

### 3. Open in browser
```
http://localhost:8080/login.html
```

### 4. Create your first account
- Go to http://localhost:8080/signup.html
- Sign up as a User OR a Doctor
- Log in and use the app

---

## MongoDB Atlas setup
1. Go to https://cloud.mongodb.com
2. Create a free cluster
3. Under "Database Access" — create a user with read/write access
4. Under "Network Access" — add 0.0.0.0/0 (allow all IPs for development)
5. Click "Connect" → "Connect your application"
6. Copy the connection string — it looks like:
   `mongodb+srv://youruser:yourpass@cluster0.xxxxx.mongodb.net/neuronudge`
7. Paste it as `spring.data.mongodb.uri` in application.properties

## Gemini API key
1. Go to https://aistudio.google.com/app/apikey
2. Click "Create API Key"
3. Paste it as `gemini.api.key` in application.properties

## Gmail App Password (for email alerts)
1. Go to your Google Account → Security → 2-Step Verification → App Passwords
2. Generate a password for "Mail"
3. Use that 16-character password (not your Gmail login password)
4. Set `app.email.enabled=true` to activate email alerts
