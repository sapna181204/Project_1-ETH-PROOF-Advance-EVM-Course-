# Project_1-ETH-PROOF-Advance-EVM-Course-
# ETH PROOF: Advance EVM Course
# CODE
/*
ETH PROOF: Advance EVM Course 
Build a simple DApp which will have a front end and Solidity Smart contracts with the following functionality:

1. An organization should be able to register themselves and their token (basically spinning off a contract for one ERC20 token).
2. Organisation should be able to mention the type of stakeholder and their vesting period (timelock).
3. Org should be able to whitelist addresses for certain stakeholders (founders, investors etc.).
4. Whitelisted addresses should be able to claim their tokens after the vesting period.

In order to pass the assessment, complete the following steps:

1. Create Solidity contracts for registering orgs and adding stakeholders for each
2. Create a front end page for users to connect their wallet.
3. Create a front end page for admins to register their org and add stakeholders + vesting details.
4. Create a page for users to be able to withdraw if they are whitelisted otherwise only org admin should be able to withdraw.
*/














// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;                                        //license under which contract is released 

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";        //Implementation of ERC20

// myToken contract
//This contract inherits ERC20 implementation and includes basics functionalities like minting and transferring tokens 
contract myToken is ERC20 {                                    //contract myToken which inherits ERC20 contract 
    constructor(string memory name, string memory symbol) 
    ERC20(name, symbol) {}                                     //initializes token with name and abbreviation 

    function mint(address to, uint amount) public {         //function mint to add new tokens 
        _mint(to, amount);                                     //add specific number of tokens to the address
    }

    function transferFromVesting(address to, uint amount)   //function handles tokens allocated
     public {
        // Implement vesting logic here
        _transfer(msg.sender, to, amount);                     //takes recipients address and number of tokens transferred 
    }
}

// OrganizationRegistry.sol
//This contract handles management of organisations, includes functionality of adding stakeholders and storing their details
contract OrganizationRegistry {                                //new contract 
    mapping(uint => Organization) public organizations;     //stores organization details by their ID
    mapping(uint => mapping(address => Stakeholder))
     public stakeholders;                             //stores stakeholders details by organisation ID and stakeholder address
    uint public organizationCount;                // keeps track of number of registered organisations 

    struct Organization {                             //struct datatype to hold organisation details 
        string name;                                  //name of organisation 
        address token;                                //stores address of organisation's token 
    }

    struct Stakeholder {                              //struct datatype to hold stakeholder's details     
        uint vestingPeriod;                        //stores vesting period
        uint stakeholderType;                      //stores type of stakeholder
        bool isWhitelisted;                           //stores whether stakeholder is whitelisted or not
    }

    function registerOrganization(string memory name, address token)           //function registers new organisation   
     public {
        organizations[organizationCount] = Organization(name, token);       //adds new organisatin in details of organisations
        organizationCount++;                                         //increment organisation count by 1  
    }

    function addStakeholder(                           //function adds new stakeholders
        uint orgId,                                 //takes organisation ID
        address stakeholder,                           //takes satkeholder's address
        uint vestingPeriod,                         //takes vesting period
        uint stakeholderType                        //takes type of stakeholder 
    ) public {
        //adds stakeholder's details to list of stakeholders and whitelisted value is set to false
        stakeholders[orgId][stakeholder] = Stakeholder(vestingPeriod, stakeholderType, false);
    }

    function whitelistAddress(uint orgId, address stakeholder) public {     //function whitelists the stakeholder's address
        stakeholders[orgId][stakeholder].isWhitelisted = true;                 //stakeholder is whitelisted 
    }
}

# README.txt 
REMIX DEFAULT WORKSPACE

Remix default workspace is present when:
i. Remix loads for the very first time 
ii. A new workspace is created with 'Default' template
iii. There are no files existing in the File Explorer

This workspace contains 3 directories:

1. 'contracts': Holds three contracts with increasing levels of complexity.
2. 'scripts': Contains four typescript files to deploy a contract. It is explained below.
3. 'tests': Contains one Solidity test file for 'Ballot' contract & one JS test file for 'Storage' contract.

SCRIPTS

The 'scripts' folder has four typescript files which help to deploy the 'Storage' contract using 'web3.js' and 'ethers.js' libraries.

For the deployment of any other contract, just update the contract's name from 'Storage' to the desired contract and provide constructor arguments accordingly 
in the file `deploy_with_ethers.ts` or  `deploy_with_web3.ts`

In the 'tests' folder there is a script containing Mocha-Chai unit tests for 'Storage' contract.

To run a script, right click on file name in the file explorer and click 'Run'. Remember, Solidity file must already be compiled.
Output from script will appear in remix terminal.

Please note, require/import is supported in a limited manner for Remix supported modules.
For now, modules supported by Remix are ethers, web3, swarmgw, chai, multihashes, remix and hardhat only for hardhat.ethers object/plugin.
For unsupported modules, an error like this will be thrown: '<module_name> module require is not supported by Remix IDE' will be shown.
