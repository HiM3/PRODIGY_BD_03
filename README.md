# Artisan Marketplace API

A comprehensive Node.js backend API for an artisan marketplace platform that connects artisans with customers. This platform allows artisans to showcase and sell their handmade products while providing customers with a seamless shopping experience.

## 🚀 Features

### User Management
- **Multi-role Authentication**: Support for users, artisans, and admins
- **Email Verification**: OTP-based email verification system
- **Secure Authentication**: JWT-based authentication with bcrypt password hashing
- **Role-based Access Control**: Different permissions for different user types

### Product Management
- **Product Catalog**: Artisans can create and manage their product listings
- **Image Upload**: Multer-based image upload system for product photos
- **Product Approval**: Admin approval system for product listings
- **Category Management**: Organized product categorization
- **Detailed Product Information**: Materials, size, description, and pricing

### Order Management
- **Shopping Cart**: Add products to cart and manage quantities
- **Order Processing**: Complete order lifecycle management
- **Order Status Tracking**: Real-time order status updates
- **Shipping Information**: Comprehensive shipping address management
- **Order History**: Complete order history for users

### Payment Integration
- **Multiple Payment Methods**: Support for various payment options
- **Stripe Integration**: Secure payment processing
- **Payment Status Tracking**: Real-time payment status updates
- **Refund Management**: Handle refunds and payment failures

### Admin Features
- **Product Approval**: Review and approve artisan products
- **User Management**: Manage user accounts and roles
- **Order Management**: Monitor and update order statuses
- **Analytics Dashboard**: Platform insights and statistics

## 🛠️ Tech Stack

- **Runtime**: Node.js
- **Framework**: Express.js
- **Database**: MongoDB with Mongoose ODM
- **Authentication**: JWT (JSON Web Tokens)
- **Password Hashing**: bcryptjs
- **File Upload**: Multer
- **Email Service**: Nodemailer
- **Payment Processing**: Stripe
- **CORS**: Cross-origin resource sharing
- **Environment Variables**: dotenv

## 📋 Prerequisites

Before running this application, make sure you have the following installed:

- Node.js (v14 or higher)
- MongoDB (local installation or MongoDB Atlas)
- npm or yarn package manager

## 🚀 Installation & Setup

### 1. Clone the Repository
```bash
git clone <repository-url>
cd task-3
```

### 2. Install Dependencies
```bash
npm install
```

### 3. Environment Configuration
Create a `.env` file in the root directory with the following variables:

```env
# Server Configuration
PORT=5000

# MongoDB Configuration
MongoDB=mongodb://localhost:27017/artisan-marketplace
# OR for MongoDB Atlas:
# MongoDB=mongodb+srv://username:password@cluster.mongodb.net/artisan-marketplace

# JWT Configuration
JWT_SECRET=your_jwt_secret_key_here

# Email Configuration (for OTP verification)
EMAIL_USER=your_email@gmail.com
EMAIL_PASS=your_email_app_password

# Stripe Configuration (for payments)
STRIPE_SECRET_KEY=sk_test_your_stripe_secret_key
STRIPE_PUBLISHABLE_KEY=pk_test_your_stripe_publishable_key

# Frontend URL (for CORS)
FRONTEND_URL=http://localhost:5173
```

### 4. Start the Server
```bash
# Development mode with nodemon
npm start

# Production mode
node server.js
```

The server will start running on `http://localhost:5000`

## 📚 API Documentation

### Authentication Endpoints

#### Register User
```http
POST /auth/register
Content-Type: application/json

{
  "username": "john_doe",
  "email": "john@example.com",
  "password": "password123",
  "gender": "male"
}
```

#### Login User
```http
POST /auth/login
Content-Type: application/json

{
  "email": "john@example.com",
  "password": "password123"
}
```

#### Verify Email OTP
```http
POST /auth/verify-otp
Content-Type: application/json

{
  "email": "john@example.com",
  "otp": "123456"
}
```

### Product Endpoints

#### Create Product (Artisan Only)
```http
POST /products/create
Authorization: Bearer <jwt_token>
Content-Type: multipart/form-data

{
  "title": "Handmade Ceramic Vase",
  "description": "Beautiful handcrafted ceramic vase",
  "materials": "Ceramic, Glaze",
  "size": "12 inches",
  "category": "Home Decor",
  "price": 89.99,
  "images": [file1, file2, ...]
}
```

#### Get All Products
```http
GET /products
```

#### Get Product by ID
```http
GET /products/:productId
```

#### Update Product (Owner Only)
```http
PUT /products/:productId
Authorization: Bearer <jwt_token>
```

#### Delete Product (Owner Only)
```http
DELETE /products/:productId
Authorization: Bearer <jwt_token>
```

### Order Endpoints

#### Create Order
```http
POST /orders/create
Authorization: Bearer <jwt_token>
Content-Type: application/json

{
  "items": [
    {
      "product": "product_id",
      "quantity": 2,
      "price": 89.99
    }
  ],
  "shippingAddress": {
    "firstName": "John",
    "lastName": "Doe",
    "street": "123 Main St",
    "city": "New York",
    "state": "NY",
    "country": "USA",
    "zipCode": "10001",
    "phone": "+1234567890"
  },
  "paymentMethod": "stripe"
}
```

#### Get User Orders
```http
GET /orders/user
Authorization: Bearer <jwt_token>
```

#### Update Order Status (Admin Only)
```http
PUT /orders/:orderId/status
Authorization: Bearer <jwt_token>
Content-Type: application/json

{
  "orderStatus": "shipped",
  "trackingNumber": "TRK123456789"
}
```

### Payment Endpoints

#### Create Payment Intent
```http
POST /payments/create-payment-intent
Authorization: Bearer <jwt_token>
Content-Type: application/json

{
  "amount": 179.98,
  "currency": "usd"
}
```

#### Confirm Payment
```http
POST /payments/confirm
Authorization: Bearer <jwt_token>
Content-Type: application/json

{
  "paymentIntentId": "pi_1234567890",
  "orderId": "order_id"
}
```

### Admin Endpoints

#### Approve Product
```http
PUT /admin/products/:productId/approve
Authorization: Bearer <jwt_token>
```

#### Get All Users
```http
GET /admin/users
Authorization: Bearer <jwt_token>
```

#### Get All Orders
```http
GET /admin/orders
Authorization: Bearer <jwt_token>
```

## 🔐 Authentication & Authorization

### JWT Token Structure
The API uses JWT tokens for authentication. Include the token in the Authorization header:
```
Authorization: Bearer <jwt_token>
```

### User Roles
- **user**: Regular customers who can browse and purchase products
- **artisan**: Product creators who can list and manage their products
- **admin**: Platform administrators with full access

### Protected Routes
Most endpoints require authentication. Some endpoints have role-based access control:
- Product creation: Artisan role required
- Admin operations: Admin role required
- Order management: User role required

## 📁 Project Structure

```
task-3/
├── app.js                 # Express app configuration
├── server.js             # Server entry point
├── package.json          # Dependencies and scripts
├── config/               # Configuration files
│   ├── db.js            # MongoDB connection
│   ├── mailer.js        # Email configuration
│   └── upload.js        # File upload configuration
├── controllers/          # Route controllers
│   ├── authController.js
│   ├── productController.js
│   ├── orderController.js
│   ├── paymentController.js
│   ├── artisanController.js
│   └── adminController.js
├── middlewares/          # Custom middlewares
│   ├── authMiddleware.js
│   └── roleMiddleware.js
├── models/              # Database models
│   ├── User.js
│   ├── Product.js
│   └── Order.js
├── routes/              # API routes
│   ├── authRoutes.js
│   ├── productRoutes.js
│   ├── orderRoutes.js
│   ├── paymentRoutes.js
│   ├── artisanRoutes.js
│   └── adminRoutes.js
├── services/            # Business logic
│   └── orderService.js
├── utils/               # Utility functions
│   └── password.js
└── uploads/             # Uploaded files
```

## 🧪 Testing

To test the API endpoints, you can use tools like:
- Postman
- Insomnia
- Thunder Client (VS Code extension)
- curl commands

### Example curl commands:

```bash
# Register a new user
curl -X POST http://localhost:5000/auth/register \
  -H "Content-Type: application/json" \
  -d '{"username":"testuser","email":"test@example.com","password":"password123","gender":"male"}'

# Login
curl -X POST http://localhost:5000/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com","password":"password123"}'

# Get all products
curl -X GET http://localhost:5000/products
```

## 🔧 Configuration

### Database Configuration
The application uses MongoDB. Make sure to:
1. Install MongoDB locally or use MongoDB Atlas
2. Set the correct MongoDB connection string in your `.env` file
3. Ensure the database is accessible

### Email Configuration
For OTP verification, configure your email settings:
1. Use Gmail or any SMTP provider
2. Enable 2-factor authentication for Gmail
3. Generate an app password
4. Update the `.env` file with your email credentials

### File Upload Configuration
- Upload directory: `./uploads/`
- Supported formats: Images (jpg, jpeg, png, gif)
- Maximum file size: 5MB per file

## 🚨 Error Handling

The API includes comprehensive error handling:
- Validation errors for request data
- Authentication and authorization errors
- Database connection errors
- File upload errors
- Payment processing errors

All errors return appropriate HTTP status codes and error messages.

## 📝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the ISC License.

## 🤝 Support

For support and questions:
- Create an issue in the repository
- Contact the development team
- Check the documentation for common issues

## 🔄 Version History

- **v1.0.0**: Initial release with basic e-commerce functionality
  - User authentication and authorization
  - Product management
  - Order processing
  - Payment integration
  - Admin dashboard

---

**Note**: This is a backend API. You'll need a frontend application to interact with these endpoints and provide a user interface. 