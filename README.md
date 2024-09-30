### Step-by-Step Guide to Set Up a Bank Management System (MERN)

---

### Step 1: Create the Main Project Directory

1. **Open your terminal** and create a new directory for your project:
   ```bash
   mkdir bank-management-system
   cd bank-management-system
   ```

2. **Initialize a new npm project** in the parent folder:
   ```bash
   npm init -y
   ```

### Step 2: Set Up the Folder Structure

Create the necessary folders for your project. Use the following commands:

```bash
mkdir -p frontend backend/database
```

### Step 3: Configure `package.json` for Workspaces

Edit the **`package.json`** file in the parent directory to include workspaces:

```json
{
  "name": "bank-management-system",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "start": "concurrently \"npm run start --workspace frontend\" \"npm run dev --workspace backend\"",
    "dev": "concurrently \"npm run start --workspace frontend\" \"npm run dev --workspace backend\"",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "description": "",
  "private": true,
  "workspaces": [
    "frontend",
    "backend"
  ]
}
```

### Step 4: Set Up the Frontend (React)

1. **Create a React application** in the **`frontend`** directory using Create React App:

   ```bash
   npx create-react-app frontend
   ```

2. **Navigate to the frontend directory**:

   ```bash
   cd frontend
   ```

3. **Install any frontend dependencies** you may need (e.g., Axios for making API requests):

   ```bash
   npm install axios react-router-dom
   ```

4. **Return to the parent directory**:

   ```bash
   cd ..
   ```

### Step 5: Set Up the Backend (Node.js/Express)

1. **Create the backend directory** and navigate into it:

   ```bash
   mkdir backend
   cd backend
   ```

2. **Initialize a Node.js project**:

   ```bash
   npm init -y
   ```

3. **Install backend dependencies**:

   ```bash
   npm install express mongoose dotenv cors body-parser
   ```

   Optionally, you can install **nodemon** for development:

   ```bash
   npm install --save-dev nodemon
   ```

4. **Create your folder structure for the backend**:

   ```bash
   mkdir config controllers models routes
   ```

5. **Create an entry file** (e.g., **`server.js`**) in the **`backend`** directory:

   ```bash
   touch server.js
   ```

### Step 6: Configure Database Folder

1. **Create database connection logic** in the **`config`** folder.

   - Create a new file, e.g., **`db.js`**:
   ```bash
   touch config/db.js
   ```

2. **Inside `db.js`**, set up your MongoDB connection:

   ```javascript
   const mongoose = require('mongoose');

   const connectDB = async () => {
     try {
       await mongoose.connect(process.env.MONGODB_URI, {
         useNewUrlParser: true,
         useUnifiedTopology: true,
       });
       console.log('MongoDB connected');
     } catch (error) {
       console.error('MongoDB connection error:', error.message);
       process.exit(1);
     }
   };

   module.exports = connectDB;
   ```

### Step 7: Configure Environment Variables

1. **Create a `.env` file** in the **`backend`** directory to store your environment variables, including your MongoDB URI:

   ```bash
   touch .env
   ```

2. **Add your MongoDB connection string** to the **`.env`** file:

   ```env
   MONGODB_URI=mongodb://localhost:27017/bank_management_system
   ```

### Step 8: Setup `server.js`

In your **`server.js`** file, set up Express and connect to MongoDB:

```javascript
const express = require('express');
const bodyParser = require('body-parser');
const cors = require('cors');
const connectDB = require('./config/db');

const app = express();

// Connect to MongoDB
connectDB();

// Middleware
app.use(cors());
app.use(bodyParser.json());

// Routes
app.get('/', (req, res) => {
  res.send('API is running...');
});

// Start the server
const PORT = process.env.PORT || 5000;
app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});
```

### Step 9: Running the Application

1. **Return to the parent directory**:
   ```bash
   cd ..
   ```

2. **Install `concurrently` to run both servers**:
   ```bash
   npm install concurrently --save-dev
   ```

3. **Run the application**:
   ```bash
   npm run dev
   ```

   This command will start both the frontend and backend servers concurrently.

### Step 10: Final Folder Structure

Your final folder structure should look like this:

```
bank-management-system/
├── node_modules/             # Single node_modules folder for both frontend and backend
├── frontend/                 # React application
│   ├── package.json
│   ├── public/
│   └── src/
├── backend/                  # Express application
│   ├── package.json
│   ├── config/               # Configuration files
│   │   └── db.js
│   ├── controllers/          # Controllers for handling requests
│   ├── models/               # Mongoose models
│   ├── routes/               # API routes
│   └── server.js             # Entry point for the backend
├── database/                 # (Optional) Database scripts or files
└── package.json              # Root package.json for workspace management
```

### Conclusion

You now have a complete setup for your **Bank Management System** using the **MERN** stack with a proper folder structure suitable for industry standards. This setup allows you to manage your frontend and backend projects efficiently in a monorepo style.