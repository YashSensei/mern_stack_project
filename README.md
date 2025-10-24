# ✨ Full Stack Realtime Chat App ✨

![Demo App](/frontend/public/screenshot-for-readme.png)

## 🌐 Live Demo

**[Visit Live App](https://mern-stack-project-vefu.onrender.com)** 🚀

[Video Tutorial on Youtube](https://youtu.be/ntKkVrQqBYY)

## 🌟 Highlights

- 🚀 **Tech Stack**: MERN (MongoDB, Express, React, Node.js) + Socket.io + TailwindCSS + DaisyUI
- 🔐 **Authentication & Authorization**: JWT with HTTP-only cookies
- � **Real-time Messaging**: Socket.io for instant message delivery
- � **Online User Status**: Live tracking of online/offline users
- 🎨 **Themes**: 32 beautiful DaisyUI themes
- 📷 **Image Sharing**: Cloudinary integration for image uploads
- 🎯 **Global State Management**: Zustand for efficient state handling
- 🐞 **Error Handling**: Comprehensive error handling on both server and client
- 📱 **Responsive Design**: Mobile-friendly UI
- ⚡ **Production Ready**: Deployment configuration included

---

## 🎯 Features

### User Features
- ✅ User registration with email and password
- ✅ Secure login with JWT authentication
- ✅ Profile management with avatar upload
- ✅ Real-time message sending and receiving
- ✅ Image sharing in conversations
- ✅ View online/offline user status
- ✅ Filter online users only
- ✅ Theme customization (32 themes)
- ✅ Message history retrieval
- ✅ Logout functionality

### Technical Features
- ✅ RESTful API endpoints
- ✅ WebSocket connections via Socket.io
- ✅ JWT token-based authentication
- ✅ Password hashing with bcrypt
- ✅ Image optimization with Cloudinary
- ✅ MongoDB database with Mongoose ODM
- ✅ CORS enabled for cross-origin requests
- ✅ Cookie-based session management

---

## � API Endpoints

### Authentication Routes (`/api/auth`)

#### POST Requests
- **POST** `/api/auth/signup`
  - **Description**: Register a new user
  - **Body**: `{ fullName, email, password }`
  - **Response**: User object with JWT token in cookie

- **POST** `/api/auth/login`
  - **Description**: Login existing user
  - **Body**: `{ email, password }`
  - **Response**: User object with JWT token in cookie

- **POST** `/api/auth/logout`
  - **Description**: Logout current user
  - **Response**: Success message

#### PUT Requests
- **PUT** `/api/auth/update-profile`
  - **Description**: Update user profile picture
  - **Auth**: Required (JWT)
  - **Body**: `{ profilePic }`
  - **Response**: Updated user object

#### GET Requests
- **GET** `/api/auth/check`
  - **Description**: Check if user is authenticated
  - **Auth**: Required (JWT)
  - **Response**: Current user object

---

### Message Routes (`/api/messages`)

#### GET Requests
- **GET** `/api/messages/users`
  - **Description**: Get all users except current user (for sidebar)
  - **Auth**: Required (JWT)
  - **Response**: Array of user objects

- **GET** `/api/messages/:id`
  - **Description**: Get message history with specific user
  - **Auth**: Required (JWT)
  - **Params**: `id` - User ID to chat with
  - **Response**: Array of message objects

#### POST Requests
- **POST** `/api/messages/send/:id`
  - **Description**: Send a message to a user
  - **Auth**: Required (JWT)
  - **Params**: `id` - Receiver's user ID
  - **Body**: `{ text, image }` (at least one required)
  - **Response**: Created message object
  - **Real-time**: Emits message via Socket.io to receiver

---

## 🔌 WebSocket Events (Socket.io)

### Client → Server Events
- **`connection`**: Triggered when client connects
  - Query param: `userId` - User's MongoDB ID
  - Stores user's socket ID in `userSocketMap`

- **`disconnect`**: Triggered when client disconnects
  - Removes user from `userSocketMap`
  - Broadcasts updated online users list

### Server → Client Events
- **`getOnlineUsers`**: Broadcast online user IDs
  - Payload: `Array<string>` - Array of user IDs
  - Triggered on: User connection/disconnection

- **`newMessage`**: Send new message to specific user
  - Payload: `Message object`
  - Target: Specific receiver's socket ID
  - Triggered when: Message is sent via POST endpoint

### WebSocket Connection Flow
1. User logs in → Frontend connects to Socket.io with `userId`
2. Server stores `{ userId: socketId }` mapping
3. Server broadcasts updated online users list
4. When message sent → Server emits to receiver's socket ID
5. Receiver gets real-time message update
6. On logout/disconnect → Socket removed, users list updated

---

## 🛠️ Installation & Setup

### Prerequisites
- Node.js (v14 or higher)
- MongoDB Atlas account (or local MongoDB)
- Cloudinary account (for image uploads)
- Git

### Step 1: Clone the Repository
```bash
git clone https://github.com/YashSensei/mern_stack_project.git
cd mern_stack_project
```

### Step 2: Setup Backend Environment Variables

Create a `.env` file in the `backend` folder:

```bash
cd backend
```

Create `.env` file with:
```env
MONGODB_URI=your_mongodb_connection_string
PORT=5001
JWT_SECRET=your_secret_key_here

CLOUDINARY_CLOUD_NAME=your_cloudinary_cloud_name
CLOUDINARY_API_KEY=your_cloudinary_api_key
CLOUDINARY_API_SECRET=your_cloudinary_api_secret

NODE_ENV=development
```

**Getting Your Credentials:**
- **MongoDB URI**: Sign up at [MongoDB Atlas](https://www.mongodb.com/cloud/atlas) and create a cluster
- **Cloudinary**: Sign up at [Cloudinary](https://cloudinary.com/) for free image hosting
- **JWT_SECRET**: Any random secure string (e.g., use a password generator)

### Step 3: Install Dependencies

#### Install Backend Dependencies
```bash
cd backend
npm install
```

#### Install Frontend Dependencies
```bash
cd ../new_frontend
npm install
```

Or install all at once from root:
```bash
npm run build
```

### Step 4: Start the Application

#### Option A: Development Mode (Recommended for Development)

**Terminal 1 - Start Backend:**
```bash
cd backend
npm run dev
```
Backend will run on `http://localhost:5001`

**Terminal 2 - Start Frontend:**
```bash
cd new_frontend
npm run dev
```
Frontend will run on `http://localhost:5173`

#### Option B: Production Mode

```bash
# From root directory
npm run build    # Builds frontend
npm start        # Starts backend with static frontend
```
Application will run on `http://localhost:5001`

### Step 5: Access the Application

Open your browser and navigate to:
- **Development**: `http://localhost:5173`
- **Production**: `http://localhost:5001`

---

## 🚀 Quick Start Commands

```bash
# Install all dependencies
npm run build

# Start production server
npm start

# Development mode
cd backend && npm run dev        # Terminal 1
cd new_frontend && npm run dev   # Terminal 2

# Seed database with sample users (optional)
cd backend
node src/seeds/user.seed.js
```

---

## 📦 Project Structure

```
fullstack-chat-app/
├── backend/
│   ├── src/
│   │   ├── controllers/      # Request handlers
│   │   ├── models/           # MongoDB schemas
│   │   ├── routes/           # API routes
│   │   ├── middleware/       # Auth middleware
│   │   ├── lib/              # Utilities (DB, Socket.io, Cloudinary)
│   │   ├── seeds/            # Database seeders
│   │   └── index.js          # Server entry point
│   ├── .env                  # Environment variables
│   └── package.json
├── new_frontend/
│   ├── src/
│   │   ├── components/       # React components
│   │   ├── pages/            # Page components
│   │   ├── store/            # Zustand state management
│   │   ├── lib/              # Utilities
│   │   └── constants/        # App constants
│   └── package.json
└── package.json              # Root package.json
```

---

## 🧪 Testing the Application

1. **Create an account**: Click "Sign up" and register
2. **Login**: Use your credentials to log in
3. **Test real-time chat**: Open two browser windows
   - Login with different users in each
   - Send messages and see real-time updates
4. **Test image upload**: Click the image icon to share photos
5. **Test themes**: Go to Settings and try different themes
6. **Test profile update**: Go to Profile and upload an avatar

---

## 🔧 Troubleshooting

**Backend won't start:**
- Check if MongoDB URI is correct
- Ensure port 5001 is not in use
- Verify all environment variables are set

**Frontend won't connect:**
- Check if backend is running on port 5001
- Verify CORS settings in backend
- Check browser console for errors

**Images won't upload:**
- Verify Cloudinary credentials in `.env`
- Check if API key has proper permissions

**WebSocket not connecting:**
- Ensure Socket.io client version matches server
- Check if port 5001 is accessible
- Verify `userId` is being sent in query params

---

## 📝 Technologies Used

### Backend
- **Express.js** - Web framework
- **Socket.io** - Real-time WebSocket communication
- **MongoDB** - Database
- **Mongoose** - ODM for MongoDB
- **JWT** - Authentication tokens
- **bcryptjs** - Password hashing
- **Cloudinary** - Image storage
- **Cookie-parser** - Cookie handling
- **CORS** - Cross-origin resource sharing

### Frontend
- **React** - UI library
- **Vite** - Build tool
- **Socket.io-client** - WebSocket client
- **Zustand** - State management
- **React Router** - Routing
- **Axios** - HTTP client
- **TailwindCSS** - Styling
- **DaisyUI** - UI components
- **Lucide React** - Icons
- **React Hot Toast** - Notifications

---

## 📄 License

ISC

---

## 👨‍💻 Author

**YashSensei**
- GitHub: [@YashSensei](https://github.com/YashSensei)
- Repository: [mern_stack_project](https://github.com/YashSensei/mern_stack_project)

---

## 🙏 Acknowledgments

Based on the tutorial by [Video Tutorial on Youtube](https://youtu.be/ntKkVrQqBYY)
