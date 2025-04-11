App.js file 1:-

import logo from "./logo.svg";
import "./App.css";
import MCPDashboard from "./MCPDashboard";
function App() {
return (
<div className="App">
<h1 style={{ padding: "20px" }}>Welcome to MCP Dashboard</h1>
<MCPDashboard />
</div>
);
}
export default App;

MCPDasboard.js file:-

import React, { useState } from "react";

const MCPDashboard = () => {
  const [walletBalance, setWalletBalance] = useState(10000);
  const [amount, setAmount] = useState("");
  const [selectedPartner, setSelectedPartner] = useState(1);
  const [topUpAmount, setTopUpAmount] = useState("");
  const [transactionHistory, setTransactionHistory] = useState([]);

  const [pickupPartners, setPickupPartners] = useState([
    {
      id: 1,
      name: "Rahul",
      status: "Active",
      orders: { total: 10, completed: 7, pending: 3 },
      wallet: 500,
      role: "Collector",
      commission: 10,
    },
    {
      id: 2,
      name: "Anjali",
      status: "Inactive",
      orders: { total: 5, completed: 5, pending: 0 },
      wallet: 300,
      role: "Supervisor",
      commission: 15,
    },
  ]);

  const [newPartner, setNewPartner] = useState({
    name: "",
    role: "Collector",
    commission: 0,
  });

  const handleTopUp = () => {
    const amt = parseInt(topUpAmount || 0);
    if (amt > 0) {
      setWalletBalance(walletBalance + amt);
      setTransactionHistory([
        ...transactionHistory,
        {
          type: "Top-up",
          amount: amt,
          target: "MCP Wallet",
          time: new Date().toLocaleString(),
        },
      ]);
      setTopUpAmount("");
    }
  };

  const handleFundChange = (id, type) => {
    const amt = parseInt(amount || 0);
    if (amt <= 0) return;

    const updatedPartners = pickupPartners.map((partner) => {
      if (partner.id === id) {
        const newWallet =
          type === "add"
            ? partner.wallet + amt
            : Math.max(partner.wallet - amt, 0);
        return { ...partner, wallet: newWallet };
      }
      return partner;
    });

    setPickupPartners(updatedPartners);

    if (type === "add") {
      setWalletBalance(walletBalance - amt);
    } else {
      setWalletBalance(walletBalance + amt);
    }

    setTransactionHistory([
      ...transactionHistory,
      {
        type: type === "add" ? "Added to Partner" : "Deducted from Partner",
        amount: amt,
        target: pickupPartners.find((p) => p.id === id).name,
        time: new Date().toLocaleString(),
      },
    ]);

    setAmount("");
  };

  const toggleStatus = (id) => {
    const updatedPartners = pickupPartners.map((partner) => {
      if (partner.id === id) {
        return {
          ...partner,
          status: partner.status === "Active" ? "Inactive" : "Active",
        };
      }
      return partner;
    });
    setPickupPartners(updatedPartners);
  };

  const handleAddPartner = () => {
    if (!newPartner.name || newPartner.commission < 0) return;
    const newId = pickupPartners.length + 1;
    const newEntry = {
      id: newId,
      name: newPartner.name,
      status: "Active",
      orders: { total: 0, completed: 0, pending: 0 },
      wallet: 0,
      role: newPartner.role,
      commission: newPartner.commission,
    };
    setPickupPartners([...pickupPartners, newEntry]);
    setNewPartner({ name: "", role: "Collector", commission: 0 });
  };

  const deletePartner = (id) => {
    setPickupPartners(pickupPartners.filter((p) => p.id !== id));
  };

  return (
    <div style={{ padding: "20px", fontFamily: "Arial" }}>
      <h1>MCP Dashboard</h1>

      <div>
        <p>
          <strong>MCP Wallet Balance:</strong> ₹{walletBalance}
        </p>
        <input
          type="number"
          value={topUpAmount}
          onChange={(e) => setTopUpAmount(e.target.value)}
          placeholder="Add to MCP Wallet"
        />
        <button onClick={handleTopUp}>Top-Up Wallet</button>
      </div>

      {/* Add Partner */}
      <h2>Add Pickup Partner</h2>
      <input
        type="text"
        placeholder="Name"
        value={newPartner.name}
        onChange={(e) => setNewPartner({ ...newPartner, name: e.target.value })}
      />
      <select
        value={newPartner.role}
        onChange={(e) => setNewPartner({ ...newPartner, role: e.target.value })}
      >
        <option value="Collector">Collector</option>
        <option value="Supervisor">Supervisor</option>
      </select>
      <input
        type="number"
        placeholder="Commission / Order"
        value={newPartner.commission}
        onChange={(e) =>
          setNewPartner({
            ...newPartner,
            commission: parseInt(e.target.value || 0),
          })
        }
      />
      <button onClick={handleAddPartner}>Add Partner</button>

      <h2>Pickup Partners</h2>
      {pickupPartners.map((partner) => {
        const completedPercent =
          (partner.orders.completed / partner.orders.total) * 100 || 0;

        return (
          <div
            key={partner.id}
            style={{
              border: "1px solid #ccc",
              padding: "10px",
              marginBottom: "15px",
            }}
          >
            <p>
              <strong>Name:</strong> {partner.name}
            </p>
            <p>
              <strong>Role:</strong> {partner.role}
            </p>
            <p>
              <strong>Status:</strong> {partner.status}
              <button
                onClick={() => toggleStatus(partner.id)}
                style={{ marginLeft: "10px" }}
              >
                Toggle
              </button>
            </p>
            <p>
              <strong>Commission/Order:</strong> ₹{partner.commission}
            </p>
            <p>
              <strong>Wallet:</strong> ₹{partner.wallet}
            </p>
            <div>
              <button>✅ Completed: {partner.orders.completed}</button>
              <button>🕒 Pending: {partner.orders.pending}</button>
              <button>📦 Total: {partner.orders.total}</button>
            </div>

            <div
              style={{
                background: "#eee",
                height: "10px",
                borderRadius: "6px",
                margin: "10px 0",
              }}
            >
              <div
                style={{
                  width: `${completedPercent}%`,
                  background: "#4caf50",
                  height: "100%",
                  borderRadius: "6px",
                }}
              />
            </div>
            <small>{completedPercent.toFixed(0)}% Completed</small>

            <div style={{ marginTop: "10px" }}>
              <input
                type="number"
                value={partner.id === parseInt(selectedPartner) ? amount : ""}
                onChange={(e) => {
                  setSelectedPartner(partner.id);
                  setAmount(e.target.value);
                }}
                placeholder="Amount"
              />
              <button onClick={() => handleFundChange(partner.id, "add")}>
                Add Money
              </button>
              <button onClick={() => handleFundChange(partner.id, "subtract")}>
                Deduct Money
              </button>
              <button
                onClick={() => deletePartner(partner.id)}
                style={{ marginLeft: "10px", color: "red" }}
              >
                Delete Partner
              </button>
            </div>
          </div>
        );
      })}

      <h2>Transaction History</h2>
      <ul>
        {transactionHistory.map((tx, index) => (
          <li key={index}>
            [{tx.time}] {tx.type} ₹{tx.amount}{" "}
            {tx.target ? `to/from ${tx.target}` : ""}
          </li>
        ))}
      </ul>
    </div>
  );
};

export default MCPDashboard;
