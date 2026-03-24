# 🔗 Blockchain-Based Rental Agreement System

A full-stack blockchain-powered web application that auto-generates rental agreements, stores them securely on Ethereum blockchain, and provides cryptographic tamper detection — with a complete role-based login system.

---

## 📌 Project Overview

Traditional rental agreements are paper-based, easily forged, and lack transparency. This system addresses these problems by:

- Role-based login system (Landlord & Tenant)
- Auto-generating professional rental agreement PDFs
- Linking agreements to verified tenant accounts via email
- Computing SHA-256 cryptographic hash of each agreement
- Storing the hash permanently on Ethereum blockchain
- Allowing anyone to verify if an agreement has been tampered with
- Secure password storage using bcrypt hashing

---

## 🔍 Research Gap Addressed

While existing works focus on full-fledged decentralized rental ecosystems, there is limited implementation of **lightweight, automated agreement integrity verification** mechanisms suitable for real-world adoption. This project addresses this gap by proposing a blockchain-based auto-generated rental agreement system with tamper-detection capability, role-based access control, and secure user authentication.

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | HTML, CSS, JavaScript |
| Backend | Python Flask |
| Database | SQLite + Flask-SQLAlchemy |
| Authentication | Flask-Login + Flask-Bcrypt |
| Blockchain | Ethereum (Ganache local) |
| Smart Contract | Solidity 0.8.21 |
| Contract Deployment | Truffle |
| Blockchain Interaction | Web3.py |
| PDF Generation | ReportLab |
| Hashing | SHA-256 (documents) + Bcrypt (passwords) |

---

## 👥 User Roles

| Feature | Landlord | Tenant |
|---------|----------|--------|
| Register / Login | ✅ | ✅ |
| Generate Agreement | ✅ | ❌ |
| View Own Agreements | ✅ | ✅ |
| Download Agreement | ✅ | ✅ |
| Verify Agreement | ✅ | ✅ |
| Profile Page | ✅ | ✅ |

---

## 📁 Project Structure
```
rental-agreement-blockchain/
│
├── contracts/
│   └── RentalAgreement.sol       ← Solidity smart contract
│
├── migrations/
│   └── 2_deploy_rental.js        ← Truffle deployment script
│
├── static/
│   ├── style.css                 ← Stylesheet
│   └── agreements/               ← Generated PDFs stored here
│
├── templates/
│   ├── about.html                ← Landing page
│   ├── login.html                ← Login page
│   ├── register.html             ← Register page
│   ├── dashboard.html            ← User dashboard
│   ├── profile.html              ← Profile page
│   ├── index.html                ← Generate agreement page
│   └── verify.html               ← Verify agreement page
│
├── app.py                        ← Flask backend (main)
├── models.py                     ← Database models (User, AgreementRecord)
├── blockchain.py                 ← Web3 / contract interaction
├── hash_utils.py                 ← SHA-256 hashing logic
├── pdf_generator.py              ← PDF generation logic
├── truffle-config.js             ← Truffle configuration
├── rentalchain.db                ← SQLite database (auto-created)
└── requirements.txt              ← Python dependencies
```

---

## ⚙️ How to Run

### 1. Clone the repository
```bash
git clone https://github.com/YOUR_USERNAME/rental-agreement-blockchain.git
cd rental-agreement-blockchain
```

### 2. Install Python dependencies
```bash
pip install -r requirements.txt
```

### 3. Install Truffle and Ganache
```bash
npm install -g truffle ganache
```

### 4. Start Ganache (Terminal 1)
```bash
ganache --port 7545
```

### 5. Deploy smart contract (Terminal 2)
```bash
truffle migrate --network development
```
Copy the contract address from output and update `CONTRACT_ADDRESS` in `blockchain.py`

### 6. Run the Flask app
```bash
python app.py
```

### 7. Open in browser
```
http://127.0.0.1:5000
```

---

## 🔑 Key Features

### 👤 Authentication System
- Register as Landlord or Tenant
- Passwords stored as bcrypt hash (never plain text)
- Session-based login with Flask-Login
- Role-based access control on all pages

### 📄 Agreement Generation (Landlord Only)
- Enter tenant's registered email address
- System verifies tenant exists in database
- If tenant not found → error shown, asks them to register
- Auto-generates professional PDF agreement
- Computes SHA-256 hash of the document
- Stores hash permanently on Ethereum blockchain
- Agreement linked to both landlord and tenant accounts

### 📊 Dashboard
- Landlords see all agreements they created
- Tenants see all agreements shared with them
- Download PDF anytime
- Blockchain Agreement ID shown for every record

### 🔍 Tamper Detection
- Upload any agreement PDF
- Enter its Agreement ID
- System recomputes hash and compares with blockchain
- Instantly detects if document was modified after creation

---

## 🗄️ Database Structure

### Users Table
| Column | Type | Description |
|--------|------|-------------|
| id | Integer | Primary key |
| full_name | String | User's full name |
| email | String | Unique email address |
| password_hash | String | Bcrypt hashed password |
| role | String | 'landlord' or 'tenant' |
| created_at | DateTime | Registration timestamp |

### Agreements Table
| Column | Type | Description |
|--------|------|-------------|
| id | Integer | Primary key |
| landlord_id | Integer | FK → Users (landlord) |
| tenant_id | Integer | FK → Users (tenant) |
| tenant_email | String | Tenant's email address |
| pdf_filename | String | Generated PDF filename |
| sha256_hash | String | Document fingerprint |
| blockchain_agreement_id | Integer | On-chain agreement ID |
| transaction_hash | String | Ethereum transaction hash |
| block_number | Integer | Block where hash is stored |
| created_at | DateTime | Agreement creation time |

---

## 📊 How It Works
```
Tenant registers → account saved in database
        ↓
Landlord logs in → enters tenant email
        ↓
System verifies tenant exists in database
        ↓
PDF Agreement auto-generated
        ↓
SHA-256 Hash computed
        ↓
Hash stored on Ethereum blockchain
        ↓
Agreement linked to both landlord + tenant in DB
        ↓
Tenant logs in → sees agreement in dashboard
        ↓
Anyone can verify document authenticity anytime
```

---

## 📚 Literature Review Mapping

| Paper | Our Contribution |
|-------|-----------------|
| Tseng et al. — Trustworthy Rental Market | Extends with lightweight hash verification |
| Santos et al. — Blockchain Documentation | Adds auto-generation + web interface |
| Patil et al. — Decentralized Rental System | Simplifies for real-world student PoC |
| ISROSET — Smart Contract Lease | Adds tamper detection module |
| IJARSCT — Auto Generated Agreement | Implements with full working prototype + auth |

---

## 👩‍💻 Developed By

- **Name:** Anjali Prashant Walekar, Anushree Verma, Allison Suvarna, Srushti Thakur
- **Project Type:** Academic PoC (Proof of Concept)
- **Domain:** Blockchain, Web Development, Cryptography
```

Then update `requirements.txt` too since we added new libraries:
```
flask
reportlab
web3
flask-sqlalchemy
flask-login
flask-bcrypt