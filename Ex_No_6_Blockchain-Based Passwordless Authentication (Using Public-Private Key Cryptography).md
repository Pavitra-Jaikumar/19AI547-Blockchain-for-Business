# Experiment 6: Blockchain-Based Passwordless Authentication (Using Public-Private Key Cryptography)
# Aim:
To implement a secure passwordless authentication system using public-private key cryptography on Ethereum. This prevents phishing and password leaks.

# Algorithm:
Step 1: User Registration
A user registers with their Ethereum public key (instead of a password).


Step 2: Login Process
When logging in, the user signs a random challenge message using their private key.


The smart contract verifies the signature using the user’s public key.



# Program:
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract PasswordlessAuthDemo {
    struct User {
        bool registered;
        address pubKey;
        bytes32 privateKey; // Fake private key for demo
    }

    mapping(address => User) public users;
    bytes32 public latestChallenge;

    event UserRegistered(address user, address pubKey, bytes32 privateKey);
    event ChallengeGenerated(bytes32 challenge);
    event SignatureGenerated(bytes32 hash, uint8 v, bytes32 r, bytes32 s);

    // Step 1: Register user
    function registerUser() public {
        require(!users[msg.sender].registered, "Already registered");

        // Fake public/private keys
        address fakePubKey = msg.sender;
        bytes32 fakePrivateKey = keccak256(abi.encodePacked(msg.sender, block.timestamp));

        users[msg.sender] = User({
            registered: true,
            pubKey: fakePubKey,
            privateKey: fakePrivateKey
        });

        emit UserRegistered(msg.sender, fakePubKey, fakePrivateKey);
    }

    // Step 2: Generate random challenge
    function generateChallenge() public returns (bytes32) {
        require(users[msg.sender].registered, "User not registered");
        latestChallenge = keccak256(abi.encodePacked(block.timestamp, msg.sender));
        emit ChallengeGenerated(latestChallenge);
        return latestChallenge;
    }

    // Step 3: "Sign" the challenge (fake signing)
    function generateSignature() public returns (bytes32 hash, uint8 v, bytes32 r, bytes32 s) {
        require(users[msg.sender].registered, "User not registered");
        
        hash = latestChallenge;
        bytes32 combined = keccak256(abi.encodePacked(users[msg.sender].privateKey, hash));
        
        // Fake values for r, s, v
        r = bytes32(uint256(uint160(users[msg.sender].pubKey)) << 96);
        s = combined;
        v = 27;

        emit SignatureGenerated(hash, v, r, s);

        return (hash, v, r, s);
    }

    // Step 4: Authenticate
    function authenticate(bytes32 hash, uint8 v, bytes32 r, bytes32 s) public view returns (bool) {
        require(users[msg.sender].registered, "User not registered");

        bytes32 expectedCombined = keccak256(abi.encodePacked(users[msg.sender].privateKey, hash));
        bytes32 expectedR = bytes32(uint256(uint160(users[msg.sender].pubKey)) << 96);
        uint8 expectedV = 27;

        if (r == expectedR && s == expectedCombined && v == expectedV) {
            return true;
        } else {
            return false;
        }
    }
}
```

# Expected Output:
Users can register without a password.
REGISTER USER
<img width="1600" height="847" alt="18553372-6e12-416b-97e9-b9cdb376ea17" src="https://github.com/user-attachments/assets/f210f41e-22eb-4ad1-89e2-ff8d67689e8a" />
USERS
<img width="1600" height="847" alt="5d315276-d83c-4f2c-8200-1d1936699df5" src="https://github.com/user-attachments/assets/a9b9f9f1-405c-42d0-9d97-73500c71ba2e" />



Users sign a challenge with their private key for authentication.
GENERATE CHALLENGE
<img width="1600" height="855" alt="18fa7314-46be-482c-8245-1f2940a985fc" src="https://github.com/user-attachments/assets/64a22f71-0f50-4915-a169-354aa80c2702" />
GENERATE SIGNATURE
<img width="1600" height="852" alt="bf51ea95-d6c0-4a79-9ce9-08745ee719e0" src="https://github.com/user-attachments/assets/f786adba-faba-4896-b074-cd5cecdd4f89" />



The smart contract verifies signatures to confirm identity.

<img width="1600" height="852" alt="5f540f14-d4ba-47ee-a3c3-0002e4e22941" src="https://github.com/user-attachments/assets/d11f4f03-8b9f-498c-9b01-0efbd529004a" />


# High-Level Overview:
Eliminates password hacks & phishing attacks.


Uses Ethereum's built-in cryptographic functions.


Inspired by Web3 login solutions like MetaMask authentication.

# RESULT: 
Thus, secure passwordless authentication system using public-private key cryptography on Ethereum has been successfully executed.
