# 🎨 Pet Clinic UI Components Guide

## Modern UI Framework Setup

### DaisyUI + Tailwind CSS Configuration

```bash
# Install dependencies
npm install -D tailwindcss postcss autoprefixer daisyui
npm install lucide-react react-hot-toast framer-motion
```

### tailwind.config.js
```javascript
module.exports = {
  content: [
    "./src/**/*.{js,jsx,ts,tsx}",
  ],
  theme: {
    extend: {
      colors: {
        'primary-teal': '#14b8a6',
        'secondary-coral': '#fb7185',
        'accent-purple': '#a855f7',
      }
    },
  },
  plugins: [require("daisyui")],
  daisyui: {
    themes: [
      {
        petclinic: {
          "primary": "#14b8a6",
          "secondary": "#fb7185",
          "accent": "#a855f7",
          "neutral": "#1f2937",
          "base-100": "#ffffff",
          "info": "#3abff8",
          "success": "#36d399",
          "warning": "#fbbd23",
          "error": "#f87272",
        },
      },
      "dark",
      "cupcake",
    ],
  },
}
```

## 📱 Component Examples

### 1. Modern Dashboard Card
```jsx
// StatsCard.jsx
import { TrendingUp, TrendingDown } from 'lucide-react';

const StatsCard = ({ title, value, change, icon: Icon, color }) => {
  const isPositive = change > 0;
  
  return (
    <div className="card bg-base-100 shadow-xl hover:shadow-2xl transition-all duration-300">
      <div className="card-body">
        <div className="flex items-center justify-between">
          <div>
            <p className="text-sm opacity-70">{title}</p>
            <p className="text-3xl font-bold mt-2">{value}</p>
            <div className="flex items-center mt-2 gap-1">
              {isPositive ? (
                <TrendingUp className="w-4 h-4 text-success" />
              ) : (
                <TrendingDown className="w-4 h-4 text-error" />
              )}
              <span className={`text-sm ${isPositive ? 'text-success' : 'text-error'}`}>
                {Math.abs(change)}%
              </span>
            </div>
          </div>
          <div className={`p-4 rounded-full bg-${color}-100`}>
            <Icon className={`w-8 h-8 text-${color}-600`} />
          </div>
        </div>
      </div>
    </div>
  );
};
```

### 2. Beautiful Pet Card
```jsx
// PetCard.jsx
import { Heart, Calendar, MapPin } from 'lucide-react';

const PetCard = ({ pet }) => {
  return (
    <div className="card card-compact bg-base-100 shadow-xl hover:scale-105 transition-transform duration-300">
      <figure className="relative">
        <img 
          src={pet.photo || '/placeholder-pet.jpg'} 
          alt={pet.name}
          className="h-48 w-full object-cover"
        />
        <div className="badge badge-secondary absolute top-2 right-2">
          {pet.species}
        </div>
      </figure>
      <div className="card-body">
        <h2 className="card-title">
          {pet.name}
          <div className="badge badge-ghost">{pet.breed}</div>
        </h2>
        <div className="flex flex-col gap-2 text-sm">
          <div className="flex items-center gap-2">
            <Calendar className="w-4 h-4 text-primary" />
            <span>Age: {pet.age} years</span>
          </div>
          <div className="flex items-center gap-2">
            <MapPin className="w-4 h-4 text-primary" />
            <span>Owner: {pet.ownerName}</span>
          </div>
        </div>
        <div className="card-actions justify-end mt-4">
          <button className="btn btn-primary btn-sm">View Details</button>
          <button className="btn btn-ghost btn-sm">
            <Heart className="w-4 h-4" />
          </button>
        </div>
      </div>
    </div>
  );
};
```

### 3. Appointment Calendar Component
```jsx
// AppointmentCalendar.jsx
import { ChevronLeft, ChevronRight, Plus } from 'lucide-react';

const AppointmentCalendar = () => {
  return (
    <div className="card bg-base-100 shadow-xl">
      <div className="card-body">
        <div className="flex justify-between items-center mb-4">
          <h2 className="card-title">Appointments</h2>
          <div className="join">
            <button className="join-item btn btn-sm">
              <ChevronLeft className="w-4 h-4" />
            </button>
            <button className="join-item btn btn-sm">Today</button>
            <button className="join-item btn btn-sm">
              <ChevronRight className="w-4 h-4" />
            </button>
          </div>
        </div>
        
        <div className="grid grid-cols-7 gap-2">
          {/* Calendar Grid */}
          {['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'].map(day => (
            <div key={day} className="text-center text-sm font-semibold p-2">
              {day}
            </div>
          ))}
          
          {/* Sample Calendar Days */}
          {[...Array(35)].map((_, i) => (
            <div 
              key={i} 
              className="aspect-square border rounded-lg p-1 hover:bg-primary/10 cursor-pointer transition-colors"
            >
              <div className="text-xs">{(i % 31) + 1}</div>
              {i === 15 && (
                <div className="w-2 h-2 bg-primary rounded-full mx-auto mt-1"></div>
              )}
            </div>
          ))}
        </div>
        
        <button className="btn btn-primary mt-4">
          <Plus className="w-4 h-4" />
          New Appointment
        </button>
      </div>
    </div>
  );
};
```

### 4. Modern Form with Validation
```jsx
// PetRegistrationForm.jsx
import { Save, Camera, User, Phone, Mail } from 'lucide-react';

const PetRegistrationForm = () => {
  return (
    <div className="card bg-base-100 shadow-xl">
      <div className="card-body">
        <h2 className="card-title">Register New Pet</h2>
        
        <div className="divider"></div>
        
        {/* Photo Upload */}
        <div className="form-control">
          <label className="label">
            <span className="label-text">Pet Photo</span>
          </label>
          <div className="flex items-center gap-4">
            <div className="avatar">
              <div className="w-24 rounded-xl border-2 border-dashed border-primary">
                <img src="/placeholder-pet.jpg" alt="Pet" />
              </div>
            </div>
            <button className="btn btn-outline btn-primary">
              <Camera className="w-4 h-4" />
              Upload Photo
            </button>
          </div>
        </div>
        
        {/* Pet Information */}
        <div className="grid md:grid-cols-2 gap-4 mt-4">
          <div className="form-control">
            <label className="label">
              <span className="label-text">Pet Name</span>
              <span className="label-text-alt text-error">*</span>
            </label>
            <input 
              type="text" 
              placeholder="Enter pet name" 
              className="input input-bordered input-primary"
            />
          </div>
          
          <div className="form-control">
            <label className="label">
              <span className="label-text">Species</span>
              <span className="label-text-alt text-error">*</span>
            </label>
            <select className="select select-bordered select-primary">
              <option disabled selected>Select species</option>
              <option>Dog</option>
              <option>Cat</option>
              <option>Bird</option>
              <option>Rabbit</option>
              <option>Other</option>
            </select>
          </div>
          
          <div className="form-control">
            <label className="label">
              <span className="label-text">Breed</span>
            </label>
            <input 
              type="text" 
              placeholder="Enter breed" 
              className="input input-bordered"
            />
          </div>
          
          <div className="form-control">
            <label className="label">
              <span className="label-text">Birth Date</span>
            </label>
            <input 
              type="date" 
              className="input input-bordered"
            />
          </div>
        </div>
        
        <div className="divider">Owner Information</div>
        
        {/* Owner Information */}
        <div className="grid md:grid-cols-2 gap-4">
          <div className="form-control">
            <label className="label">
              <span className="label-text">Owner Name</span>
              <span className="label-text-alt text-error">*</span>
            </label>
            <div className="input-group">
              <span className="bg-primary text-primary-content">
                <User className="w-4 h-4" />
              </span>
              <input 
                type="text" 
                placeholder="Full name" 
                className="input input-bordered w-full"
              />
            </div>
          </div>
          
          <div className="form-control">
            <label className="label">
              <span className="label-text">Phone Number</span>
              <span className="label-text-alt text-error">*</span>
            </label>
            <div className="input-group">
              <span className="bg-primary text-primary-content">
                <Phone className="w-4 h-4" />
              </span>
              <input 
                type="tel" 
                placeholder="09XX XXX XXXX" 
                className="input input-bordered w-full"
              />
            </div>
          </div>
          
          <div className="form-control md:col-span-2">
            <label className="label">
              <span className="label-text">Email Address</span>
            </label>
            <div className="input-group">
              <span className="bg-primary text-primary-content">
                <Mail className="w-4 h-4" />
              </span>
              <input 
                type="email" 
                placeholder="email@example.com" 
                className="input input-bordered w-full"
              />
            </div>
          </div>
        </div>
        
        {/* Actions */}
        <div className="card-actions justify-end mt-6">
          <button className="btn btn-ghost">Cancel</button>
          <button className="btn btn-primary">
            <Save className="w-4 h-4" />
            Register Pet
          </button>
        </div>
      </div>
    </div>
  );
};
```

### 5. Beautiful Data Table
```jsx
// AppointmentsTable.jsx
import { Search, Filter, Download, Eye, Edit, Trash2 } from 'lucide-react';

const AppointmentsTable = () => {
  return (
    <div className="card bg-base-100 shadow-xl">
      <div className="card-body">
        {/* Header */}
        <div className="flex flex-col md:flex-row justify-between items-start md:items-center gap-4">
          <h2 className="card-title">Recent Appointments</h2>
          <div className="flex gap-2">
            <div className="join">
              <input 
                className="input input-bordered join-item" 
                placeholder="Search..."
              />
              <button className="btn join-item btn-primary">
                <Search className="w-4 h-4" />
              </button>
            </div>
            <button className="btn btn-ghost">
              <Filter className="w-4 h-4" />
            </button>
            <button className="btn btn-ghost">
              <Download className="w-4 h-4" />
            </button>
          </div>
        </div>
        
        {/* Table */}
        <div className="overflow-x-auto mt-4">
          <table className="table table-zebra">
            <thead>
              <tr>
                <th>
                  <label>
                    <input type="checkbox" className="checkbox" />
                  </label>
                </th>
                <th>Pet</th>
                <th>Owner</th>
                <th>Veterinarian</th>
                <th>Date & Time</th>
                <th>Status</th>
                <th>Actions</th>
              </tr>
            </thead>
            <tbody>
              <tr className="hover">
                <th>
                  <label>
                    <input type="checkbox" className="checkbox" />
                  </label>
                </th>
                <td>
                  <div className="flex items-center space-x-3">
                    <div className="avatar">
                      <div className="mask mask-squircle w-12 h-12">
                        <img src="/pet1.jpg" alt="Pet" />
                      </div>
                    </div>
                    <div>
                      <div className="font-bold">Max</div>
                      <div className="text-sm opacity-50">Golden Retriever</div>
                    </div>
                  </div>
                </td>
                <td>
                  Juan Dela Cruz
                  <br/>
                  <span className="badge badge-ghost badge-sm">0917-123-4567</span>
                </td>
                <td>Dr. Garcia</td>
                <td>Nov 5, 2024 - 10:00 AM</td>
                <td>
                  <div className="badge badge-success">Completed</div>
                </td>
                <td>
                  <div className="flex gap-1">
                    <button className="btn btn-ghost btn-xs">
                      <Eye className="w-4 h-4" />
                    </button>
                    <button className="btn btn-ghost btn-xs">
                      <Edit className="w-4 h-4" />
                    </button>
                    <button className="btn btn-ghost btn-xs text-error">
                      <Trash2 className="w-4 h-4" />
                    </button>
                  </div>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
        
        {/* Pagination */}
        <div className="flex justify-center mt-4">
          <div className="join">
            <button className="join-item btn btn-sm">«</button>
            <button className="join-item btn btn-sm btn-active">1</button>
            <button className="join-item btn btn-sm">2</button>
            <button className="join-item btn btn-sm">3</button>
            <button className="join-item btn btn-sm">»</button>
          </div>
        </div>
      </div>
    </div>
  );
};
```

### 6. Modern Navigation Sidebar
```jsx
// Sidebar.jsx
import { 
  Home, Calendar, Users, Heart, Package, 
  FileText, Settings, LogOut, ChevronDown 
} from 'lucide-react';

const Sidebar = () => {
  return (
    <div className="drawer-side">
      <label htmlFor="drawer-toggle" className="drawer-overlay"></label>
      <aside className="w-64 min-h-full bg-base-200">
        {/* Logo */}
        <div className="flex items-center gap-2 p-4 border-b">
          <div className="avatar">
            <div className="w-10 rounded-full bg-primary">
              <span className="text-2xl">🐾</span>
            </div>
          </div>
          <span className="text-xl font-bold">Pet Clinic</span>
        </div>
        
        {/* Navigation */}
        <ul className="menu p-4">
          <li>
            <a className="active">
              <Home className="w-5 h-5" />
              Dashboard
            </a>
          </li>
          
          <li>
            <details open>
              <summary>
                <Users className="w-5 h-5" />
                Clients & Pets
              </summary>
              <ul>
                <li><a>All Clients</a></li>
                <li><a>All Pets</a></li>
                <li><a>Add New</a></li>
              </ul>
            </details>
          </li>
          
          <li>
            <a>
              <Calendar className="w-5 h-5" />
              Appointments
              <div className="badge badge-secondary">5</div>
            </a>
          </li>
          
          <li>
            <a>
              <Heart className="w-5 h-5" />
              Medical Records
            </a>
          </li>
          
          <li>
            <a>
              <Package className="w-5 h-5" />
              Inventory
              <div className="badge badge-warning">Low</div>
            </a>
          </li>
          
          <li>
            <a>
              <FileText className="w-5 h-5" />
              Reports
            </a>
          </li>
          
          <div className="divider"></div>
          
          <li>
            <a>
              <Settings className="w-5 h-5" />
              Settings
            </a>
          </li>
          
          <li>
            <a className="text-error">
              <LogOut className="w-5 h-5" />
              Logout
            </a>
          </li>
        </ul>
        
        {/* User Profile */}
        <div className="absolute bottom-0 w-full p-4 border-t">
          <div className="flex items-center gap-3">
            <div className="avatar online">
              <div className="w-10 rounded-full">
                <img src="/user-avatar.jpg" alt="User" />
              </div>
            </div>
            <div>
              <p className="font-semibold">Dr. Garcia</p>
              <p className="text-xs opacity-60">Veterinarian</p>
            </div>
          </div>
        </div>
      </aside>
    </div>
  );
};
```

### 7. Alert & Notification Components
```jsx
// NotificationExamples.jsx
import { CheckCircle, XCircle, AlertCircle, Info } from 'lucide-react';

const NotificationExamples = () => {
  return (
    <>
      {/* Success Alert */}
      <div className="alert alert-success shadow-lg">
        <CheckCircle className="w-6 h-6" />
        <div>
          <h3 className="font-bold">Appointment Confirmed!</h3>
          <div className="text-xs">Pet Max has been scheduled for Nov 5, 2024</div>
        </div>
        <button className="btn btn-sm">View</button>
      </div>
      
      {/* Error Alert */}
      <div className="alert alert-error shadow-lg">
        <XCircle className="w-6 h-6" />
        <span>Failed to save medical record. Please try again.</span>
      </div>
      
      {/* Warning Alert */}
      <div className="alert alert-warning shadow-lg">
        <AlertCircle className="w-6 h-6" />
        <span>Low inventory: Rabies Vaccine (5 units remaining)</span>
      </div>
      
      {/* Info Toast */}
      <div className="toast toast-top toast-end">
        <div className="alert alert-info">
          <Info className="w-6 h-6" />
          <span>New appointment request received</span>
        </div>
      </div>
    </>
  );
};
```

## 🎨 Theme Switcher Component
```jsx
// ThemeSwitcher.jsx
import { Sun, Moon, Palette } from 'lucide-react';

const ThemeSwitcher = () => {
  return (
    <div className="dropdown dropdown-end">
      <label tabIndex={0} className="btn btn-ghost btn-circle">
        <Palette className="w-5 h-5" />
      </label>
      <ul tabIndex={0} className="dropdown-content menu p-2 shadow bg-base-100 rounded-box w-52">
        <li><a data-theme="petclinic">🏥 Pet Clinic</a></li>
        <li><a data-theme="light">☀️ Light</a></li>
        <li><a data-theme="dark">🌙 Dark</a></li>
        <li><a data-theme="cupcake">🧁 Cupcake</a></li>
      </ul>
    </div>
  );
};
```

## 📱 Mobile Responsive Layout
```jsx
// MainLayout.jsx
const MainLayout = ({ children }) => {
  return (
    <div className="drawer drawer-mobile">
      <input id="drawer-toggle" type="checkbox" className="drawer-toggle" />
      
      <div className="drawer-content flex flex-col">
        {/* Navbar */}
        <div className="navbar bg-base-100 shadow-lg lg:hidden">
          <div className="flex-none">
            <label htmlFor="drawer-toggle" className="btn btn-square btn-ghost">
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" className="inline-block w-6 h-6 stroke-current">
                <path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M4 6h16M4 12h16M4 18h16"></path>
              </svg>
            </label>
          </div>
          <div className="flex-1">
            <a className="btn btn-ghost normal-case text-xl">🐾 Pet Clinic</a>
          </div>
        </div>
        
        {/* Main Content */}
        <main className="flex-1 p-4 lg:p-8">
          {children}
        </main>
      </div>
      
      <Sidebar />
    </div>
  );
};
```

## 🎯 Loading States
```jsx
// LoadingStates.jsx
const LoadingStates = () => {
  return (
    <>
      {/* Skeleton Loader for Cards */}
      <div className="card bg-base-100 shadow-xl">
        <div className="card-body">
          <div className="animate-pulse">
            <div className="h-4 bg-base-300 rounded w-3/4 mb-4"></div>
            <div className="h-8 bg-base-300 rounded w-1/2 mb-2"></div>
            <div className="h-4 bg-base-300 rounded w-full mb-2"></div>
            <div className="h-4 bg-base-300 rounded w-5/6"></div>
          </div>
        </div>
      </div>
      
      {/* Spinner */}
      <div className="flex justify-center items-center h-64">
        <span className="loading loading-spinner loading-lg text-primary"></span>
      </div>
      
      {/* Progress Bar */}
      <progress className="progress progress-primary w-full" value="70" max="100"></progress>
    </>
  );
};
```

## 🚀 Animation Examples with Framer Motion
```jsx
// AnimatedComponents.jsx
import { motion } from 'framer-motion';

const AnimatedCard = ({ children }) => {
  return (
    <motion.div
      initial={{ opacity: 0, y: 20 }}
      animate={{ opacity: 1, y: 0 }}
      transition={{ duration: 0.3 }}
      whileHover={{ scale: 1.02 }}
      className="card bg-base-100 shadow-xl"
    >
      {children}
    </motion.div>
  );
};

const StaggeredList = ({ items }) => {
  const container = {
    hidden: { opacity: 0 },
    show: {
      opacity: 1,
      transition: {
        staggerChildren: 0.1
      }
    }
  };
  
  const item = {
    hidden: { opacity: 0, x: -20 },
    show: { opacity: 1, x: 0 }
  };
  
  return (
    <motion.ul
      variants={container}
      initial="hidden"
      animate="show"
      className="space-y-2"
    >
      {items.map((listItem, i) => (
        <motion.li key={i} variants={item} className="p-4 bg-base-200 rounded">
          {listItem}
        </motion.li>
      ))}
    </motion.ul>
  );
};
```