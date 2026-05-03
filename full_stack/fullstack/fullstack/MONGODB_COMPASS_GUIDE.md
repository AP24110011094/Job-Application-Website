# MongoDB Compass Connection Guide

## 🔗 Quick Connection Methods

### Method 1: Local MongoDB Connection
1. **Start MongoDB Service:**
   - Windows: Open Services → Find MongoDB Server → Right-click → Start
   - Or run: `net start MongoDB`

2. **Open MongoDB Compass**
3. **Connection String:**
   ```
   mongodb://localhost:27017
   ```
4. **Click Connect**

### Method 2: MongoDB Atlas (Cloud Connection)

#### Step 1: Get Connection String from Atlas
1. Go to https://cloud.mongodb.com
2. Log in to your account
3. Navigate to "Database" → "Clusters"
4. Click "Connect" button
5. Select "Compass" option
6. Copy the connection string

#### Step 2: Connect via Compass
1. Open MongoDB Compass
2. Click "New Connection"
3. Paste the connection string
4. Click "Connect"

**Example Atlas Connection String:**
```
mongodb+srv://your_username:your_password@cluster0.abcde.mongodb.net/talentflow?retryWrites=true&w=majority
```

---

## 🔐 Connection String Breakdown

```
mongodb+srv://username:password@cluster.mongodb.net/database?retryWrites=true&w=majority
                 │           │              │              │              │        │
                 │           │              │              │              │        └─ Write concern
                 │           │              │              │              └────── Retry writes
                 │           │              │              └─────────────────── Database name
                 │           │              └──────────────────────────────── Host
                 │           └──────────────────────────────────────────────── Password
                 └──────────────────────────────────────────────────────────── Username
```

---

## ✅ Verification Steps

### Check Backend Connection to MongoDB

1. **Start Backend:**
   ```bash
   cd backend
   npm run dev
   ```

2. **Look for Success Message:**
   ```
   MongoDB connected: localhost
   ```

3. **If Error:**
   ```
   MongoDB connection failed: getaddrinfo ENOTFOUND localhost
   ```
   **Fix:** Ensure MongoDB service is running

### Check Frontend Connection to Backend

1. **Start Frontend:**
   ```bash
   cd frontend
   npm run dev
   ```

2. **Open Browser DevTools (F12)**

3. **Try to Register:**
   - Fill registration form
   - Click submit
   - Check Network tab → Should show POST request to http://localhost:5000/api/auth/register

4. **Check MongoDB Compass:**
   - Navigate to `talentflow` database
   - Check `users` collection
   - You should see the registered user

---

## 📊 MongoDB Compass Features

### Navigation
- **Databases:** Left sidebar shows all databases
- **Collections:** Click database to expand and see collections
- **Documents:** Click collection to view/edit documents
- **Indexes:** View and create indexes for performance

### Common Actions

**View All Documents:**
1. Click on collection
2. All documents appear in main panel
3. Click a document to expand details

**Search Documents:**
1. Click on collection
2. Use filter bar at top
3. Example: `{ "email": "user@example.com" }`

**Edit Document:**
1. Click on document
2. Click "Edit Document" button
3. Make changes
4. Click "Update"

**Delete Document:**
1. Click on document
2. Click "Delete" button
3. Confirm deletion

---

## 🎯 Expected Database Structure

After running the application, your MongoDB should have:

```
talentflow (database)
├── users (collection)
│   ├── _id
│   ├── name
│   ├── email
│   ├── password (hashed)
│   ├── role
│   └── createdAt
│
├── jobs (collection)
│   ├── _id
│   ├── title
│   ├── description
│   ├── requirements
│   ├── salary
│   └── createdAt
│
├── candidates (collection)
│   ├── _id
│   ├── name
│   ├── email
│   ├── resume
│   ├── status
│   └── createdAt
│
├── interviews (collection)
│   ├── _id
│   ├── candidateId
│   ├── jobId
│   ├── date
│   ├── feedback
│   └── createdAt
│
└── feedback (collection)
    ├── _id
    ├── interviewId
    ├── ratings
    ├── comments
    └── createdAt
```

---

## 🐛 Common Connection Issues & Fixes

| Issue | Cause | Solution |
|-------|-------|----------|
| Can't connect to localhost | MongoDB not running | Start MongoDB service |
| Authentication failed (Atlas) | Wrong password or IP not whitelisted | Check credentials, whitelist IP |
| Connection timeout | Network issue or firewall | Check firewall, test network |
| "talentflow" database doesn't exist | First time running | Create data by registering user |
| Can't see new data | Not refreshed | Click refresh button in Compass |

---

## 🚀 Production MongoDB Atlas Setup

### 1. Create Atlas Account
```
https://www.mongodb.com/cloud/atlas → Sign Up → Create Free Cluster
```

### 2. Security Settings
- **Network Access:** Add your IP or `0.0.0.0/0` (testing only)
- **Database Access:** Create username/password
- **IP Whitelist:** Production = specific IPs only

### 3. Get Connection String
```
Cluster → Connect → Choose Connection Method → Copy Connection String
```

### 4. Update Environment Variable
```env
# backend/.env
MONGO_URI=mongodb+srv://username:password@cluster0.xxxxx.mongodb.net/talentflow?retryWrites=true&w=majority
```

### 5. Restart Backend
```bash
npm run dev
```

---

## 💡 Tips for MongoDB Compass Usage

1. **Use Filters for Large Collections:**
   ```json
   { "status": "active" }
   ```

2. **Sort Documents:**
   - Click column header → Choose Ascending/Descending

3. **Export Data:**
   - Right-click collection → Export Full Collection

4. **Create New Collections:**
   - Click "+" next to Collections
   - Enter collection name

5. **Schema Analysis:**
   - Click "Schema" tab in collection view
   - See field distribution

---

## 🎓 MongoDB Query Examples

### Find User by Email
```javascript
db.users.findOne({ email: "user@example.com" })
```

### Count Active Jobs
```javascript
db.jobs.countDocuments({ status: "active" })
```

### Get Candidates by Status
```javascript
db.candidates.find({ status: "shortlisted" })
```

### Update Interview Date
```javascript
db.interviews.updateOne(
  { _id: ObjectId("...") },
  { $set: { date: new Date() } }
)
```

### Delete Completed Interviews
```javascript
db.interviews.deleteMany({ status: "completed" })
```

---

## 📞 Getting Help

- **MongoDB Docs:** https://docs.mongodb.com/
- **Compass Docs:** https://docs.mongodb.com/compass/
- **Connection Issues:** Check MongoDB logs in `/var/log/mongodb/`
- **Atlas Support:** https://support.mongodb.com/

---

## ✨ You're All Set!

Your MongoDB is now connected and ready to use with TalentFlow ATS. Start the application and begin managing your recruitment process! 🎉
