# How to Run NeuroNudge

## The ONLY correct way to access the app

1. Start the Spring Boot backend in IntelliJ
2. Open your browser and go to: **http://localhost:8080/login.html**

DO NOT open HTML files by double-clicking in file manager.
The `file://` protocol cannot make API calls to the backend.

---

## Step-by-step setup

### 1. Clone the repository

```bash
git clone https://github.com/mehulprajapati2610/neuronudge.git
cd neuronudge
```

---

### 2. Create your `.env` file

Go to the `backend` folder and create a file called `.env`:

```
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/neuronudge
JWT_SECRET=your_jwt_secret_here
GROQ_API_KEY=your_groq_api_key_here
GMAIL_USERNAME=your_gmail@gmail.com
GMAIL_PASSWORD=your_16_char_app_password
GOOGLE_CLIENT_ID=your_google_oauth_client_id
```

You can use `.env.example` in the root as a reference.

---

### 3. Copy frontend files into Spring Boot static folder

```bash
cp -r frontend/* backend/src/main/resources/static/
```

On Windows:
```
xcopy frontend\* backend\src\main\resources\static\ /E /Y
```

---

### 4. Run in IntelliJ

- Open the `backend` folder as a Maven project in IntelliJ
- Run `NeuroNudgeApplication.java`
- Wait for `Started NeuroNudgeApplication` in the console

---

### 5. Open in browser

```
http://localhost:8080/login.html
```

### 6. Create your first account

- Go to `http://localhost:8080/signup.html`
- Sign up as a **User** OR a **Doctor**
- Log in and use the app

---

## MongoDB Atlas setup

1. Go to [cloud.mongodb.com](https://cloud.mongodb.com)
2. Create a free cluster
3. Under **Database Access** — create a user with read/write access
4. Under **Network Access** — add `0.0.0.0/0` (allow all IPs for development)
5. Click **Connect** → **Connect your application**
6. Copy the connection string:
   ```
   mongodb+srv://youruser:yourpass@cluster0.xxxxx.mongodb.net/neuronudge
   ```
7. Paste it as `MONGODB_URI` in your `.env` file

---

## Groq API key

1. Go to [console.groq.com](https://console.groq.com)
2. Login → **API Keys** → **Create API Key**
3. Copy the key and paste it as `GROQ_API_KEY` in your `.env` file

---

## Gmail App Password (for email alerts)

1. Go to your Google Account → **Security** → **2-Step Verification** → **App Passwords**
2. Generate a password for "Mail"
3. Use that 16-character password — not your actual Gmail password
4. Paste it as `GMAIL_PASSWORD` in your `.env` file

---

## Google OAuth setup

1. Go to [console.cloud.google.com](https://console.cloud.google.com)
2. Create a project → **APIs & Services** → **Credentials**
3. Create an **OAuth 2.0 Client ID**
4. Add `http://localhost:8080` to Authorised JavaScript Origins
5. Copy the Client ID and paste it as `GOOGLE_CLIENT_ID` in your `.env` file