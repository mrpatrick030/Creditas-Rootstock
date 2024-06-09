# P2PLending Smart Contract

## Overview

The `P2PLending` contract enables peer-to-peer lending with ERC20 tokens and ERC721 NFTs as collateral. It manages loan creation, funding, repayment, and collateral claiming.

## Constructor

### Initialization

Initializes the contract by setting the owner, treasury address, and accepted collateral tokens.

```solidity
constructor(
    address _initialowner,
    address _treasuryaddr,
    address ult,
    address usdt,
    address dai
) Ownable(_initialowner) {
    treasuryAddress = _treasuryaddr;
    _addCollateral(ult);
    _addCollateral(usdt);
    _addCollateral(dai);
}
```

- **Sets Initial Owner**: Calls `Ownable` constructor with `_initialowner`.
- **Sets Treasury Address**: Sets `treasuryAddress` to `_treasuryaddr`.
- **Adds Initial Collaterals**: Adds `ult`, `usdt`, and `dai` as accepted collaterals.

## Functions

### setadaoaddress

Sets the DAO address which has certain administrative privileges.

```solidity
function setadaoaddress(address _dao) public onlyOwner {
    dao = _dao;
}
```

- **Ownership Restriction**: Can only be called by the contract owner (`onlyOwner`).
- **Sets DAO Address**: Updates the `dao` state variable.

### addCollateral

Adds a new collateral type to the list of accepted collaterals.

```solidity
function addCollateral(address _collateral) public onlyOwner {
    _addCollateral(_collateral);
    emit CollateralAdded(_collateral);
}
```

- **Ownership Restriction**: Can only be called by the contract owner.
- **Internal Logic**: Calls `_addCollateral` to handle the actual logic.
- **Event Emission**: Emits `CollateralAdded` event.

### _addCollateral (Internal Function)

Handles the logic of adding a new collateral.

```solidity
function _addCollateral(address _collateral) internal {
    accepteddCollaterals[_collateral] = true;
    accepted_collaterals.push(_collateral);
}
```

- **Updates Mapping**: Marks the token as accepted in `accepteddCollaterals`.
- **Updates Array**: Adds the token to `accepted_collaterals`.

### createLoan

Allows a borrower to create a new loan request.

```solidity
function createLoan(
    uint _amount,
    uint _interest,
    uint _duration,
    uint _collateralamount,
    address _collateral,
    bool _isERC20,
    uint _fundingDeadline
) external payable isCollateral(_collateral) {
    require(_amount >= MIN_LOAN_AMOUNT && _amount <= MAX_LOAN_AMOUNT, "Loan amount must be between MIN_LOAN_AMOUNT and MAX_LOAN_AMOUNT");
    require(_interest >= MIN_INTEREST_RATE && _interest <= MAX_INTEREST_RATE, "Interest rate must be between MIN_INTEREST_RATE and MAX_INTEREST_RATE");
    require(_duration > 0, "Loan duration must be greater than 0");
    require(outstanding[msg.sender] == false, "settle outstanding loan");

    uint loanId = loanCount++;
    uint _repaymentAmount = _amount + (_amount * _interest) / 100;

    loans[loanId] = Loan({
        loan_id: loanId,
        amount: _amount,
        interest: _interest,
        duration: _duration + block.timestamp,
        repaymentAmount: _repaymentAmount,
        fundingDeadline: _fundingDeadline + block.timestamp,
        collateralAmount: _collateralamount,
        borrower: msg.sender,
        lender: payable(address(0)),
        collateral: _collateral,
        isCollateralErc20: _isERC20,
        active: true,
        repaid: false
    });

    if (_isERC20) {
        require(IERC20(_collateral).transferFrom(msg.sender, address(this), _collateralamount), "ERC20 transfer failed");
    } else {
        IERC721(_collateral).transferFrom(msg.sender, address(this), _collateralamount);
    }

    emit LoanCreated(loanId, _amount, _interest, _duration, _fundingDeadline, msg.sender, address(0));
}
```

- **Parameter Validation**: Ensures loan amount and interest rate are within allowed ranges. Checks loan duration is positive and borrower has no outstanding loans.
- **Repayment Calculation**: Calculates the total repayment amount including interest.
- **Loan Creation**: Creates and stores a new loan record in the `loans` mapping.
- **Collateral Transfer**: Transfers the specified collateral amount from borrower to contract.
  - Uses `IERC20.transferFrom` for ERC20 tokens.
  - Uses `IERC721.transferFrom` for ERC721 tokens.
- **Event Emission**: Emits `LoanCreated` event.

### fundLoan

Allows a lender to fund an active loan.

```solidity
function fundLoan(uint _loanId) external payable onlyActiveLoan(_loanId) {
    Loan storage loan = loans[_loanId];
    require(msg.sender != loan.borrower, "Borrower cannot fund their own loan");

    if (block.timestamp > loan.fundingDeadline) {
        loan.active = false;
        revert("deadline passed");
    }

    payable(loan.borrower).transfer(msg.value);
    loan.lender = payable(msg.sender);
    outstanding[loan.borrower] = true;

    emit LoanFunded(_loanId, msg.sender, msg.value);
}
```

- **Active Loan Check**: Ensures the loan is active.
- **Self-Funding Restriction**: Prevents borrower from funding their own loan.
- **Deadline Check**: Verifies the current time is within the funding deadline.
- **Funds Transfer**: Transfers the loan amount to the borrower's address.
- **Loan Update**: Updates the loan's lender and marks it as funded.
- **Event Emission**: Emits `LoanFunded` event.

### repayLoan

Allows the borrower to repay the loan.

```solidity
function repayLoan(uint _loanId) external payable onlyActiveLoan(_loanId) onlyBorrower(_loanId) {
    Loan storage loan = loans[_loanId];
    require(!loan.repaid, "Loan has already been repaid");

    uint interestAmount = (loan.amount * loan.interest) / 100;
    uint repaymentAmount = loan.amount + interestAmount;
    uint serviceFee = (repaymentAmount * SERVICE_FEE_PERCENTAGE) / 100;
    uint amountAfterFee = repaymentAmount - serviceFee;

    loan.lender.transfer(amountAfterFee);
    payable(treasuryAddress).transfer(serviceFee);

    if (loan.isCollateralErc20) {
        require(IERC20(loan.collateral).transfer(msg.sender, loan.collateralAmount), "Failed to transfer ERC20 collateral");
    } else {
        IERC721(loan.collateral).transferFrom(address(this), msg.sender, loan.collateralAmount);
    }

    totalServiceCharges += serviceFee;
    emit LoanRepaid(_loanId, repaymentAmount);
    emit ServiceFeeDeducted(_loanId, serviceFee);

    loan.repaid = true;
    outstanding[loan.borrower] = false;
    loan.active = false;
}
```

- **Active Loan and Borrower Check**: Ensures the loan is active and the caller is the borrower.
- **Repayment Validation**: Ensures the loan is not already repaid.
- **Repayment Calculation**: Calculates total repayment amount including interest.
- **Service Fee Deduction**: Calculates and deducts the service fee from the repayment amount.
- **Funds Transfer**: Transfers the remaining amount to the lender and the service fee to the treasury.
- **Collateral Return**: Returns the collateral to the borrower.
  - Uses `IERC20.transfer` for ERC20 tokens.
  - Uses `IERC721.transferFrom` for ERC721 tokens.
- **Service Charges Update**: Accumulates the service fee.
- **Event Emission**: Emits `LoanRepaid` and `ServiceFeeDeducted` events.
- **Loan Update**: Marks the loan as repaid and inactive.

### claimCollateral

Allows the lender to claim the collateral if the loan defaults.

```solidity
function claimCollateral(uint _loanId) external onlyActiveLoan(_loanId) {
    Loan storage loan = loans[_loanId];

    require(block.timestamp > loan.fundingDeadline && !loan.repaid, "Loan is still active or already repaid");
    require(msg.sender == loan.lender, "Only the lender can claim collateral");

    if (loan.isCollateralErc20) {
        require(IERC20(loan.collateral).transfer(msg.sender, loan.collateralAmount), "Failed to transfer ERC20 collateral");
    } else {
        IERC721(loan.collateral).transferFrom(address(this), msg.sender, loan.collateralAmount);
    }

    loan.active = false;
    defaulters[loan.borrower] += 1;
    outstanding[loan.borrower] = false;

    emit CollateralClaimed(_loanId, msg.sender);
}
```

- **Active Loan Check**: Ensures the loan is active.
- **Default Check**: Verifies the loan is past its funding deadline and not repaid.
- **Lender Check**: Confirms that the caller is the lender.
- **Collateral Transfer**: Transfers the collateral to the lender.
  - Uses `IERC20.transfer` for ERC20 tokens.
  - Uses `IERC721.transferFrom` for ERC721 tokens.
- **Loan Update**: Marks the loan as inactive and the borrower as a

 defaulter.
- **Event Emission**: Emits `CollateralClaimed` event.

### withdrawFunds

Allows the borrower to withdraw their collateral if the loan is not funded.

```solidity
function withdrawFunds(uint _loanId) external {
    Loan storage loan = loans[_loanId];
    require(loan.collateralAmount > 0, "loan does not exist");
    require(block.timestamp > loan.fundingDeadline, "funding deadline not reached");

    if (loan.isCollateralErc20) {
        require(IERC20(loan.collateral).transfer(msg.sender, loan.collateralAmount), "ERC20 transfer failed");
    } else {
        IERC721(loan.collateral).transferFrom(address(this), msg.sender, loan.collateralAmount);
    }

    loan.active = false;
    loan.collateralAmount = 0;
    loan.borrower = address(0);
    loan.collateral = address(0);
}
```

- **Collateral Existence Check**: Ensures the loan has collateral.
- **Funding Deadline Check**: Verifies the funding deadline has passed.
- **Collateral Transfer**: Transfers the collateral back to the borrower.
  - Uses `IERC20.transfer` for ERC20 tokens.
  - Uses `IERC721.transferFrom` for ERC721 tokens.
- **Loan Update**: Marks the loan as inactive and clears the collateral details.

### getAllLoans

Fetches all loan records.

```solidity
function getAllLoans() public view returns (Loan[] memory) {
    Loan[] memory result = new Loan[](loanCount);
    for (uint i = 0; i < loanCount; i++) {
        result[i] = loans[i];
    }
    return result;
}
```

- **Loan Array Creation**: Creates an array of all loans.
- **Loan Retrieval**: Iterates through the `loans` mapping and populates the array.
- **Return**: Returns the array containing all loans.

### getLoanInfo

Fetches information about a specific loan.

```solidity
function getLoanInfo(uint _loanId) public view returns (Loan memory) {
    return loans[_loanId];
}
```

- **Return Loan**: Returns the loan struct corresponding to `_loanId`.

## Events

Events are emitted to allow external systems to react to contract state changes.

- `LoanCreated`: Emitted when a loan is created.
- `LoanFunded`: Emitted when a loan is funded.
- `LoanRepaid`: Emitted when a loan is repaid.
- `ServiceFeeDeducted`: Emitted when the service fee is deducted.
- `ServiceChargesWithdrawn`: Emitted when service charges are withdrawn.
- `CollateralClaimed`: Emitted when collateral is claimed by the lender.
- `CollateralAdded`: Emitted when a new collateral is added.

```solidity
event LoanCreated(uint loanId, uint amount, uint interest, uint duration, uint fundingDeadline, address borrower, address lender);
event LoanFunded(uint loanId, address lender, uint amount);
event LoanRepaid(uint loanId, uint repaymentAmount);
event ServiceFeeDeducted(uint loanId, uint serviceFee);
event ServiceChargesWithdrawn(uint amount);
event CollateralClaimed(uint loanId, address lender);
event CollateralAdded(address collateral);
```

---