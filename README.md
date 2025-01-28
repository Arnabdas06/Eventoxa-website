import React from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { Tabs, TabsList, TabsTrigger, TabsContent } from "@/components/ui/tabs";
import { useState } from "react";
import { motion } from "framer-motion";

const LoginPage = ({ onLogin, onCreateAccount }) => {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");

  return (
    <div className="flex flex-col items-center justify-center h-screen bg-gradient-to-r from-red-500 to-blue-500">
      <Card className="w-96 p-4">
        <CardContent>
          <Tabs defaultValue="customer">
            <TabsList className="flex justify-between">
              <TabsTrigger value="customer">Customer</TabsTrigger>
              <TabsTrigger value="vendor">Vendor</TabsTrigger>
            </TabsList>
            <TabsContent value="customer">
              <h2 className="text-xl font-bold mb-4">Login as Customer</h2>
              <Input placeholder="Email" className="mb-2" value={email} onChange={(e) => setEmail(e.target.value)} />
              <Input placeholder="Password" type="password" className="mb-4" value={password} onChange={(e) => setPassword(e.target.value)} />
              <Button className="w-full" onClick={() => onLogin('customer', email)}>Login</Button>
              <Button variant="link" className="mt-2" onClick={onCreateAccount}>Create New Account</Button>
            </TabsContent>
            <TabsContent value="vendor">
              <h2 className="text-xl font-bold mb-4">Login as Vendor</h2>
              <Input placeholder="Email" className="mb-2" value={email} onChange={(e) => setEmail(e.target.value)} />
              <Input placeholder="Password" type="password" className="mb-4" value={password} onChange={(e) => setPassword(e.target.value)} />
              <Button className="w-full" onClick={() => onLogin('vendor', email)}>Login</Button>
              <Button variant="link" className="mt-2" onClick={onCreateAccount}>Create New Account</Button>
            </TabsContent>
          </Tabs>
        </CardContent>
      </Card>
    </div>
  );
};

const CreateAccountPage = ({ onAccountCreated }) => (
  <div className="h-screen bg-gradient-to-r from-blue-500 to-red-500 flex items-center justify-center">
    <Card className="w-96 p-4">
      <CardContent>
        <h2 className="text-2xl font-bold text-center mb-4">Create New Account</h2>
        <Input placeholder="Full Name" className="mb-2" />
        <Input placeholder="Email" className="mb-2" />
        <Input placeholder="Password" type="password" className="mb-2" />
        <Input placeholder="Confirm Password" type="password" className="mb-4" />
        <Button className="w-full" onClick={onAccountCreated}>Create Account</Button>
      </CardContent>
    </Card>
  </div>
);

const CustomerHomePage = ({ onSearch, onViewAccount }) => (
  <div className="h-screen bg-blue-100 p-4">
    <h1 className="text-3xl font-bold text-center mt-4">Welcome to EventOxa!</h1>
    <div className="max-w-3xl mx-auto mt-8 space-y-4">
      <Input placeholder="Search for vendors" className="w-full" />
      <div className="flex space-x-4">
        <Input placeholder="Budget" className="flex-1" />
        <Input placeholder="Event Type" className="flex-1" />
      </div>
      <Button className="w-full" onClick={onSearch}>Search</Button>
    </div>
    <div className="absolute top-4 right-4">
      <Button onClick={onViewAccount}>Account Info</Button>
    </div>
  </div>
);

const AccountInfoPage = ({ userEmail }) => (
  <div className="h-screen bg-white p-8">
    <h1 className="text-3xl font-bold text-center text-red-600">Account Details</h1>
    <div className="max-w-lg mx-auto mt-8 space-y-4">
      <p className="text-xl text-red-600">Email: {userEmail}</p>
      <p className="text-xl text-red-600">EventOxa Support: <a href="mailto:Eventoxa2@gmail.com" className="underline">Eventoxa2@gmail.com</a></p>
    </div>
  </div>
);

const VendorProfilePage = ({ onSaveProfile }) => (
  <div className="h-screen bg-red-100">
    <h1 className="text-3xl font-bold text-center mt-8">Create Your Vendor Profile</h1>
    <form className="max-w-lg mx-auto mt-8 space-y-4">
      <Input placeholder="Vendor Name" className="w-full" />
      <Input placeholder="Description of Services" className="w-full" />
      <Input placeholder="Contact Information" className="w-full" />
      <Input placeholder="Portfolio Link" className="w-full" />
      <Input placeholder="Fees" type="number" className="w-full" />
      <Button className="w-full" onClick={onSaveProfile}>Save Profile</Button>
    </form>
  </div>
);

const VendorList = ({ vendors, onSelectVendor }) => (
  <div className="h-screen bg-blue-50 p-4">
    <h1 className="text-2xl font-bold text-center mt-4">Search Results</h1>
    <div className="max-w-3xl mx-auto mt-8 space-y-4">
      {vendors.map((vendor, index) => (
        <Card key={index} className="p-4">
          <CardContent>
            <h2 className="text-xl font-bold text-red-600">{vendor.name}</h2>
            <p className="text-red-600">{vendor.description}</p>
            <p className="text-red-600">Fees: {vendor.fees}</p>
            <Button className="mt-2" onClick={() => onSelectVendor(vendor)}>Book Now</Button>
          </CardContent>
        </Card>
      ))}
    </div>
  </div>
);

const PaymentPage = ({ vendor, onPay }) => (
  <div className="h-screen bg-green-100 p-4">
    <h1 className="text-3xl font-bold text-center mt-4">Payment</h1>
    <div className="max-w-lg mx-auto mt-8">
      <p className="text-xl">You are booking: {vendor.name}</p>
      <p className="text-xl">Total Fees: {vendor.fees}</p>
      <p className="text-xl">Payable Now: {vendor.fees * 0.3}</p>
      <Button className="w-full mt-4" onClick={onPay}>Pay 30%</Button>
    </div>
  </div>
);

const EventOxaApp = () => {
  const [userType, setUserType] = useState(null);
  const [userEmail, setUserEmail] = useState(null);
  const [creatingAccount, setCreatingAccount] = useState(false);
  const [viewingAccount, setViewingAccount] = useState(false);
  const [vendors, setVendors] = useState([
    { name: "Vendor A", description: "Photography Services", fees: 1000 },
    { name: "Vendor B", description: "Catering Services", fees: 2000 },
  ]);
  const [selectedVendor, setSelectedVendor] = useState(null);

  const handleLogin = (type, email) => {
    setUserType(type);
    setUserEmail(email);
  };

  const handleCreateAccount = () => {
    setCreatingAccount(true);
  };

  const handleAccountCreated = () => {
    setCreatingAccount(false);
    alert("Account created successfully! You can now log in.");
  };

  const handleSearch = () => {
    alert("Search functionality coming soon!");
  };

  const handleSelectVendor = (vendor) => {
    setSelectedVendor(vendor);
  };

  const handlePay = () => {
    alert("Payment successful!");
    setSelectedVendor(null);
  };

  const handleViewAccount = () => {
    setViewingAccount(true);
  };

  if (creatingAccount) {
    return <CreateAccountPage onAccountCreated={handleAccountCreated} />;
  }

  if (viewingAccount) {
    return <AccountInfoPage userEmail={userEmail} />;
  }

  if (!userType) {
    return <LoginPage onLogin={handleLogin} onCreateAccount={handleCreateAccount} />;
  }

  if (userType === "customer") {
    if (selectedVendor) {
      return <PaymentPage vendor={selectedVendor} onPay={handlePay} />;
    }
    return <CustomerHomePage onSearch={handleSearch} onViewAccount={handleViewAccount} />;
  }

  if (userType === "vendor") {
    return <VendorProfilePage onSaveProfile={() => alert("Profile saved!")} />;
  }

  return null;
};

export default EventOxaApp;
const VendorProfilePage = ({ onSaveProfile }) => {
  const [vendorData, setVendorData] = useState({
    name: "",
    description: "",
    contact: "",
    portfolio: "",
    fees: "",
  });

  const handleInputChange = (e) => {
    const { name, value } = e.target;
    setVendorData((prev) => ({ ...prev, [name]: value }));
  };

  return (
    <div className="h-screen bg-red-100">
      <h1 className="text-3xl font-bold text-center mt-8">Create Your Vendor Profile</h1>
      <form
        className="max-w-lg mx-auto mt-8 space-y-4"
        onSubmit={(e) => {
          e.preventDefault();
          onSaveProfile(vendorData);
        }}
      >
        <Input name="name" placeholder="Vendor Name" className="w-full" onChange={handleInputChange} />
        <Input name="description" placeholder="Description of Services" className="w-full" onChange={handleInputChange} />
        <Input name="contact" placeholder="Contact Information" className="w-full" onChange={handleInputChange} />
        <Input name="portfolio" placeholder="Portfolio Link" className="w-full" onChange={handleInputChange} />
        <Input name="fees" type="number" placeholder="Fees" className="w-full" onChange={handleInputChange} />
        <Button type="submit" className="w-full">Save Profile</Button>
      </form>
    </div>
  );
};

const CustomerHomePage = ({ vendors, onSearch, onViewAccount }) => (
  <div className="h-screen bg-blue-100 p-4">
    <h1 className="text-3xl font-bold text-center mt-4">Welcome to EventOxa!</h1>
    <div className="max-w-3xl mx-auto mt-8 space-y-4">
      <Input placeholder="Search for vendors" className="w-full" />
      <div className="flex space-x-4">
        <Input placeholder="Budget" className="flex-1" />
        <Input placeholder="Event Type" className="flex-1" />
      </div>
      <Button className="w-full" onClick={onSearch}>Search</Button>
    </div>
    <div className="absolute top-4 right-4">
      <Button onClick={onViewAccount}>Account Info</Button>
    </div>
    <div className="mt-8">
      <h2 className="text-2xl font-bold">Vendor Profiles</h2>
      <div className="mt-4 space-y-4">
        {vendors.map((vendor, index) => (
          <Card key={index} className="p-4">
            <CardContent>
              <h2 className="text-xl font-bold text-red-600">{vendor.name}</h2>
              <p className="text-red-600">{vendor.description}</p>
              <p className="text-red-600">Fees: {vendor.fees}</p>
              <Button className="mt-2">Book Now</Button>
            </CardContent>
          </Card>
        ))}
      </div>
    </div>
  </div>
);

const EventOxaApp = () => {
  const [userType, setUserType] = useState(null);
  const [userEmail, setUserEmail] = useState(null);
  const [creatingAccount, setCreatingAccount] = useState(false);
  const [viewingAccount, setViewingAccount] = useState(false);
  const [vendors, setVendors] = useState([]);
  const [selectedVendor, setSelectedVendor] = useState(null);

  const handleLogin = (type, email) => {
    setUserType(type);
    setUserEmail(email);
  };

  const handleCreateAccount = () => {
    setCreatingAccount(true);
  };

  const handleAccountCreated = () => {
    setCreatingAccount(false);
    alert("Account created successfully! You can now log in.");
  };

  const handleSearch = () => {
    alert("Search functionality coming soon!");
  };

  const handleSaveProfile = (vendorData) => {
    setVendors((prev) => [...prev, vendorData]);
    alert("Profile saved successfully!");
  };

  const handleViewAccount = () => {
    setViewingAccount(true);
  };

  if (creatingAccount) {
    return <CreateAccountPage onAccountCreated={handleAccountCreated} />;
  }

  if (viewingAccount) {
    return <AccountInfoPage userEmail={userEmail} />;
  }

  if (!userType) {
    return <LoginPage onLogin={handleLogin} onCreateAccount={handleCreateAccount} />;
  }

  if (userType === "customer") {
    return <CustomerHomePage vendors={vendors} onSearch={handleSearch} onViewAccount={handleViewAccount} />;
  }

  if (userType === "vendor") {
    return <VendorProfilePage onSaveProfile={handleSaveProfile} />;
  }

  return null;
};

export default EventOxaApp; 
