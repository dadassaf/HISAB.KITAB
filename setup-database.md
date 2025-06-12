# MongoDB Atlas Setup Guide

Follow these steps to set up your MongoDB Atlas database for the SplitWise app:

## 1. Create MongoDB Atlas Account

1. Go to [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
2. Sign up for a free account or log in if you already have one

## 2. Create a New Cluster

1. Click "Create a New Cluster"
2. Choose the **FREE** tier (M0 Sandbox)
3. Select a cloud provider and region (choose one closest to you)
4. Give your cluster a name (e.g., "splitwise-cluster")
5. Click "Create Cluster" (this may take a few minutes)

## 3. Create Database User

1. In the Atlas dashboard, go to "Database Access" in the left sidebar
2. Click "Add New Database User"
3. Choose "Password" authentication method
4. Enter a username (e.g., "splitwise-user")
5. Generate a secure password (save this!)
6. Under "Database User Privileges", select "Read and write to any database"
7. Click "Add User"

## 4. Configure Network Access

1. Go to "Network Access" in the left sidebar
2. Click "Add IP Address"
3. For development, click "Allow Access from Anywhere" (0.0.0.0/0)
   - **Note**: For production, you should restrict this to specific IP addresses
4. Click "Confirm"

## 5. Get Connection String

1. Go to "Clusters" in the left sidebar
2. Click "Connect" on your cluster
3. Choose "Connect your application"
4. Select "Node.js" as the driver and version "4.1 or later"
5. Copy the connection string (it will look like this):
   ```
   mongodb+srv://<username>:<password>@cluster0.xxxxx.mongodb.net/?retryWrites=true&w=majority
   ```

## 6. Update Your .env File

1. Open your `.env` file in the project root
2. Replace the placeholder values in the MONGO_URI:

```env
MONGO_URI=mongodb+srv://your_username:your_password@cluster0.xxxxx.mongodb.net/splitwise?retryWrites=true&w=majority
JWT_SECRET=your_super_secret_jwt_key_here_make_it_long_and_secure
PORT=5051
NODE_ENV=development
```

**Important**: 
- Replace `your_username` with the database username you created
- Replace `your_password` with the password you generated
- Replace `cluster0.xxxxx.mongodb.net` with your actual cluster URL
- Add `/splitwise` before the `?` to specify the database name
- If your password contains special characters, you need to URL encode them:
  - `@` becomes `%40`
  - `#` becomes `%23`
  - `$` becomes `%24`
  - `%` becomes `%25`
  - etc.

## 7. Test the Connection

1. Save your `.env` file
2. Start your server:
   ```bash
   npm run server
   ```
3. You should see:
   ```
   🔄 Connecting to MongoDB...
   ✅ MongoDB Connected: cluster0-xxxxx.mongodb.net
   📊 Database: splitwise
   🚀 Server running on port 5051
   ```

## 8. Verify Database Collections

Once you start using the app:
1. Go back to MongoDB Atlas
2. Click "Browse Collections" on your cluster
3. You should see the `splitwise` database with collections:
   - `users`
   - `groups` 
   - `expenses`

## Troubleshooting

### Common Issues:

1. **Authentication Failed**
   - Double-check your username and password
   - Make sure you're using the database user credentials, not your Atlas account credentials

2. **Network Timeout**
   - Verify your IP address is whitelisted in Network Access
   - Check if you're behind a corporate firewall

3. **Invalid Connection String**
   - Make sure you've replaced all placeholder values
   - Ensure special characters in password are URL encoded

4. **Database Not Found**
   - The database will be created automatically when you first insert data
   - Make sure you've added `/splitwise` to your connection string

### Testing Your Setup:

You can test your database connection by running:

```bash
node -e "
const mongoose = require('mongoose');
require('dotenv').config();
mongoose.connect(process.env.MONGO_URI)
  .then(() => console.log('✅ Database connection successful!'))
  .catch(err => console.error('❌ Database connection failed:', err.message));
"
```

## Security Best Practices

1. **Never commit your .env file** - it's already in .gitignore
2. **Use strong passwords** for database users
3. **Restrict IP access** in production
4. **Rotate credentials** regularly
5. **Use environment-specific databases** (dev, staging, production)

Your MongoDB Atlas database is now ready for the SplitWise app!