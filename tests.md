```
digitalax-contracts/smart-contracts$ yarn test
yarn run v1.22.4
$ npx buidler test
Compiling...
Downloading compiler version 0.6.12


contracts/DigitalaxGenesisNFT.sol:260:5: Warning: Function state mutability can be restricted to pure
    function _getMaxGenesisContributionTokens() internal virtual view returns (uint256) {
    ^ (Relevant source part starts here and spans across multiple lines).

Compiled 42 contracts successfully


  Contract: DigitalaxAccessControls
    MINTER_ROLE
      ✓ should allow admin to add minters
      ✓ should allow admin to remove minters
      ✓ should revert if not admin
      ✓ should revert even if minter is adding already a minter
      ✓ should revert if does not have the correct role (41ms)
    SMART_CONTRACT_ROLE
      ✓ should allow admin to add smart contracts
      ✓ should allow admin to remove contracts
      ✓ should revert if not admin when adding
      ✓ should revert even if smart_contract is adding already a smart_contract
      ✓ should revert if not admin when removing
    DEFAULT_ADMIN_ROLE
      ✓ should allow admin to add admin
      ✓ should allow admin to remove admin
      ✓ should revert if already has minter role
      ✓ should revert if does not have the correct role

  Contract: DigitalaxAuction scenario tests
    scenario 1: happy path creation, auction and burn
      ✓ Garment and children are created
      Given an auction
        ✓ the auction is resulted properly and token ownership assigned
        Given a user burns it
          ✓ user destroys parent token and is assigned the composite children (101ms)
    scenario 2: happy path creation, auction and increasing balance post purchase
      ✓ Garment and children are created
      Given an auction
        ✓ the auction is resulted properly and token ownership assigned
        Given user now tops up there existing child balances
          ✓ balances are updated (74ms)
          ✓ cannot top-up receive parent tokens is msg.data is not 32 bytes in size (boolean) (46ms)
          ✓ cannot top-up receive parent tokens is msg.data is not 32 bytes in size (random bytes) (47ms)
          ✓ cannot top-up existing children to someone else parent (50ms)
          ✓ cannot top-up existing children to someone else parent even with approval
          ✓ cannot add new children from the materials contract to someone else parent (50ms)
          ✓ cannot add new children from another 1155 token to another token (52ms)
    scenario 3: happy path creation, can add additional children up to max children allowed
      ✓ Garment and children are created
      ✓ should fail when trying to batch send more tokens than limit (135ms)
      ✓ is fine sending children up to the limit, after this it fails (87ms)
    scenario 4: topping up multiple children to a parent but the children are a mixture of new and old children
      ✓ sending a mix of new and existing children (95ms)

  Contract: DigitalaxAuction
    Contract deployment
      ✓ Reverts when access controls is zero (38ms)
      ✓ Reverts when garment is zero (40ms)
      ✓ Reverts when platform fee recipient is zero (42ms)
    Admin functions
      Auction resulting
        ✓ Successfully results the auction (45ms)
      updateMinBidIncrement()
        ✓ fails when not admin
        ✓ successfully updates min bid
      updateBidWithdrawalLockTime()
        ✓ fails when not admin
        ✓ successfully updates min bid
      updateAuctionReservePrice()
        ✓ fails when not admin
        ✓ fails when auction doesnt exist
        ✓ successfully updates auction reserve
      updateAuctionStartTime()
        ✓ fails when not admin
        ✓ fails when auction does not exist
        ✓ successfully updates auction start time
      updateAuctionEndTime()
        ✓ fails when not admin
        ✓ fails when no auction exists
        ✓ fails when wnd time must be greater than start
        ✓ fails when end time has passed
        ✓ successfully updates auction end time
      updateAccessControls()
        ✓ fails when not admin
        ✓ reverts when trying to set recipient as ZERO address
        ✓ successfully updates access controls
      updatePlatformFee()
        ✓ fails when not admin
        ✓ successfully updates access controls
      updatePlatformFeeRecipient()
        ✓ reverts when not admin
        ✓ reverts when trying to set recipient as ZERO address
        ✓ successfully updates platform fee recipient
      toggleIsPaused()
        ✓ can successfully toggle as admin
        ✓ reverts when not admin
    createAuction()
      validation
        ✓ fails if does not have minter role
        ✓ fails if endTime is in the past
        ✓ fails if endTime greater than startTime
        ✓ fails if token already has auction in play (42ms)
        ✓ fails if you dont own the token (55ms)
        ✓ fails if token does not exist
        ✓ fails if contract is paused
      successful creation
        ✓ Token retains in the ownership of the auction creator (51ms)
      creating using real contract (not mock)
        ✓ can successfully create (83ms)
    createAuctionOnBehalfOfOwner()
      validation
        ✓ fails when sender does not have admin or smart contract role
        ✓ fails when auction does not have approval for garment
      successful creation
        ✓ succeeds with admin role
        ✓ succeeds with smart contract role
    placeBid()
      validation
        ✓ will revert if sender is smart contract
        ✓ will fail with 721 token not on auction
        ✓ will fail with valid token but no auction
        ✓ will fail when auction finished
        ✓ will fail when contract is paused
        ✓ will fail when outbidding someone by less than the increment
      successfully places bid
        ✓ places bid and you are the top owner
        ✓ will refund the top bidder if found (38ms)
        ✓ successfully increases bid
    withdrawBid()
      ✓ fails with withdrawing a bid which does not exist
      ✓ fails with withdrawing a bid which you did not make
      ✓ fails with withdrawing when lockout time not passed
      ✓ fails when withdrawing after auction end
      ✓ fails when the contract is paused
      ✓ successfully withdraw the bid
    resultAuction()
      validation
        ✓ cannot result if not an admin
        ✓ cannot result if auction has not ended
        ✓ cannot result if auction does not exist
        ✓ cannot result if the auction is reserve not reached (40ms)
        ✓ cannot result if the auction has no winner
        ✓ cannot result if there is no approval (46ms)
        ✓ cannot result if the auction if its already resulted (67ms)
      successfully resulting an auction
        ✓ transfer token to the winner (60ms)
        ✓ transfer funds to the token creator and platform (53ms)
        ✓ transfer funds to the token to only the creator when reserve meet directly (43ms)
        ✓ records primary sale price on garment NFT (47ms)
    cancelAuction()
      validation
        ✓ cannot cancel if not an admin
        ✓ cannot cancel if auction already cancelled (45ms)
        ✓ cannot cancel if auction already resulted (77ms)
        ✓ cannot cancel if auction does not exist
        ✓ Can cancel as smart contract
        ✓ Cancel clears down auctions and top bidder
        ✓ funds are sent back to the highest bidder if found
        ✓ no funds transferred if no bids
    create, cancel and re-create an auction
      ✓ once created and then cancelled, can be created and resulted properly (139ms)

  Contract: DigitalaxGarmentFactory
    createNewChild()
      ✓ Creates a new strand successfully
      ✓ Reverts when sender is not a minter
    createNewChildren()
      ✓ Creates a multiple strands successfully
      ✓ Reverts when sender is not a minter
    mintParentWithChildren()
      ✓ Can mint and link a strand (98ms)
      ✓ Reverts when sender does not have the minter role
    mintParentWithoutChildren()
      ✓ Can mint parent without children
      ✓ Reverts when sender does not have the minter role

  Contract: Digitalax Garment Sale
    ✓ Can setup a garment sale (142ms)

  Contract: Core ERC721 tests for DigitalaxGarmentNFT
    Contract interface
      ERC165
        ERC165's supportsInterface(bytes4)
          ✓ uses less than 30k gas
          ✓ claims support
        supportsInterface(bytes4)
          ✓ has to be implemented
      ERC721
        ERC165's supportsInterface(bytes4)
          ✓ uses less than 30k gas
          ✓ claims support
        balanceOf(address)
          ✓ has to be implemented
        ownerOf(uint256)
          ✓ has to be implemented
        approve(address,uint256)
          ✓ has to be implemented
        getApproved(uint256)
          ✓ has to be implemented
        setApprovalForAll(address,bool)
          ✓ has to be implemented
        isApprovedForAll(address,address)
          ✓ has to be implemented
        transferFrom(address,address,uint256)
          ✓ has to be implemented
        safeTransferFrom(address,address,uint256)
          ✓ has to be implemented
        safeTransferFrom(address,address,uint256,bytes)
          ✓ has to be implemented
      ERC721Enumerable
        ERC165's supportsInterface(bytes4)
          ✓ uses less than 30k gas
          ✓ claims support
        totalSupply()
          ✓ has to be implemented
        tokenOfOwnerByIndex(address,uint256)
          ✓ has to be implemented
        tokenByIndex(uint256)
          ✓ has to be implemented
      ERC721Metadata
        ERC165's supportsInterface(bytes4)
          ✓ uses less than 30k gas
          ✓ claims support
        name()
          ✓ has to be implemented
        symbol()
          ✓ has to be implemented
        tokenURI(uint256)
          ✓ has to be implemented
    metadata
      ✓ has a name
      ✓ has a symbol
      token URI
        ✓ reverts when queried for non existent token id
        ✓ can be set for a token id
        ✓ reverts when setting for non existent token id
        ✓ reverts when no admin or smart contract
        ✓ tokens with URI can be burnt 
    with minted tokens
      balanceOf
        when the given address owns some tokens
          ✓ returns the amount of tokens owned by the given address
        when the given address does not own any tokens
          ✓ returns 0
        when querying the zero address
          ✓ throws
      ownerOf
        when the given token ID was tracked by this token
          ✓ returns the owner of the given token ID
        when the given token ID was not tracked by this token
          ✓ reverts
      transfers
        via transferFrom
          when called by the owner
            ✓ transfers the ownership of the given token ID to the given address
            ✓ emits a Transfer event
(node:100724) DeprecationWarning: expectEvent.inLogs() is deprecated. Use expectEvent() instead.
            ✓ clears the approval for the token ID
            ✓ emits an Approval event
            ✓ adjusts owners balances
            ✓ adjusts owners tokens by index
          when called by the approved individual
            ✓ transfers the ownership of the given token ID to the given address
            ✓ emits a Transfer event
            ✓ clears the approval for the token ID
            ✓ emits an Approval event
            ✓ adjusts owners balances
            ✓ adjusts owners tokens by index
          when called by the operator
            ✓ transfers the ownership of the given token ID to the given address
            ✓ emits a Transfer event
            ✓ clears the approval for the token ID
            ✓ emits an Approval event
            ✓ adjusts owners balances
            ✓ adjusts owners tokens by index
          when called by the owner without an approved user
            ✓ transfers the ownership of the given token ID to the given address
            ✓ emits a Transfer event
            ✓ clears the approval for the token ID
            ✓ emits an Approval event
            ✓ adjusts owners balances
            ✓ adjusts owners tokens by index
          when sent to the owner
            ✓ keeps ownership of the token
            ✓ clears the approval for the token ID
            ✓ emits only a transfer event
            ✓ keeps the owner balance
            ✓ keeps same tokens by index
          when the address of the previous owner is incorrect
            ✓ reverts
          when the sender is not authorized for the token id
            ✓ reverts
          when the given token ID does not exist
            ✓ reverts
          when the address to transfer the token to is the zero address
            ✓ reverts
        via safeTransferFrom
          with data
            to a user account
              when called by the owner
                ✓ transfers the ownership of the given token ID to the given address
                ✓ emits a Transfer event
                ✓ clears the approval for the token ID
                ✓ emits an Approval event
                ✓ adjusts owners balances
                ✓ adjusts owners tokens by index
              when called by the approved individual
                ✓ transfers the ownership of the given token ID to the given address
                ✓ emits a Transfer event
                ✓ clears the approval for the token ID
                ✓ emits an Approval event
                ✓ adjusts owners balances
                ✓ adjusts owners tokens by index
              when called by the operator
                ✓ transfers the ownership of the given token ID to the given address
                ✓ emits a Transfer event
                ✓ clears the approval for the token ID
                ✓ emits an Approval event
                ✓ adjusts owners balances
                ✓ adjusts owners tokens by index
              when called by the owner without an approved user
                ✓ transfers the ownership of the given token ID to the given address
                ✓ emits a Transfer event
                ✓ clears the approval for the token ID
                ✓ emits an Approval event
                ✓ adjusts owners balances
                ✓ adjusts owners tokens by index
              when sent to the owner
                ✓ keeps ownership of the token
                ✓ clears the approval for the token ID
                ✓ emits only a transfer event
                ✓ keeps the owner balance
                ✓ keeps same tokens by index
              when the address of the previous owner is incorrect
                ✓ reverts
              when the sender is not authorized for the token id
                ✓ reverts
              when the given token ID does not exist
                ✓ reverts
              when the address to transfer the token to is the zero address
                ✓ reverts
            to a valid receiver contract
              ✓ calls onERC721Received
              ✓ calls onERC721Received from approved
              when called by the owner
                ✓ transfers the ownership of the given token ID to the given address
                ✓ emits a Transfer event
                ✓ clears the approval for the token ID
                ✓ emits an Approval event
                ✓ adjusts owners balances
                ✓ adjusts owners tokens by index
              when called by the approved individual
                ✓ transfers the ownership of the given token ID to the given address
                ✓ emits a Transfer event
                ✓ clears the approval for the token ID
                ✓ emits an Approval event
                ✓ adjusts owners balances
                ✓ adjusts owners tokens by index
              when called by the operator
                ✓ transfers the ownership of the given token ID to the given address
                ✓ emits a Transfer event
                ✓ clears the approval for the token ID
                ✓ emits an Approval event
                ✓ adjusts owners balances
                ✓ adjusts owners tokens by index
              when called by the owner without an approved user
                ✓ transfers the ownership of the given token ID to the given address
                ✓ emits a Transfer event
                ✓ clears the approval for the token ID
                ✓ emits an Approval event
                ✓ adjusts owners balances
                ✓ adjusts owners tokens by index
              when sent to the owner
                ✓ keeps ownership of the token
                ✓ clears the approval for the token ID
                ✓ emits only a transfer event
                ✓ keeps the owner balance
                ✓ keeps same tokens by index
              when the address of the previous owner is incorrect
                ✓ reverts
              when the sender is not authorized for the token id
                ✓ reverts
              when the given token ID does not exist
                ✓ reverts
              when the address to transfer the token to is the zero address
                ✓ reverts
              with an invalid token id
                ✓ reverts
          without data
            to a user account
              when called by the owner
                ✓ transfers the ownership of the given token ID to the given address
                ✓ emits a Transfer event
                ✓ clears the approval for the token ID
                ✓ emits an Approval event
                ✓ adjusts owners balances
                ✓ adjusts owners tokens by index
              when called by the approved individual
                ✓ transfers the ownership of the given token ID to the given address
                ✓ emits a Transfer event
                ✓ clears the approval for the token ID
                ✓ emits an Approval event
                ✓ adjusts owners balances
                ✓ adjusts owners tokens by index
              when called by the operator
                ✓ transfers the ownership of the given token ID to the given address
                ✓ emits a Transfer event
                ✓ clears the approval for the token ID
                ✓ emits an Approval event
                ✓ adjusts owners balances
                ✓ adjusts owners tokens by index
              when called by the owner without an approved user
                ✓ transfers the ownership of the given token ID to the given address
                ✓ emits a Transfer event
                ✓ clears the approval for the token ID
                ✓ emits an Approval event
                ✓ adjusts owners balances
                ✓ adjusts owners tokens by index
              when sent to the owner
                ✓ keeps ownership of the token
                ✓ clears the approval for the token ID
                ✓ emits only a transfer event
                ✓ keeps the owner balance
                ✓ keeps same tokens by index
              when the address of the previous owner is incorrect
                ✓ reverts
              when the sender is not authorized for the token id
                ✓ reverts
              when the given token ID does not exist
                ✓ reverts
              when the address to transfer the token to is the zero address
                ✓ reverts
            to a valid receiver contract
              ✓ calls onERC721Received
              ✓ calls onERC721Received from approved
              when called by the owner
                ✓ transfers the ownership of the given token ID to the given address
                ✓ emits a Transfer event
                ✓ clears the approval for the token ID
                ✓ emits an Approval event
                ✓ adjusts owners balances
                ✓ adjusts owners tokens by index
              when called by the approved individual
                ✓ transfers the ownership of the given token ID to the given address
                ✓ emits a Transfer event
                ✓ clears the approval for the token ID
                ✓ emits an Approval event
                ✓ adjusts owners balances
                ✓ adjusts owners tokens by index
              when called by the operator
                ✓ transfers the ownership of the given token ID to the given address
                ✓ emits a Transfer event
                ✓ clears the approval for the token ID
                ✓ emits an Approval event
                ✓ adjusts owners balances
                ✓ adjusts owners tokens by index
              when called by the owner without an approved user
                ✓ transfers the ownership of the given token ID to the given address
                ✓ emits a Transfer event
                ✓ clears the approval for the token ID
                ✓ emits an Approval event
                ✓ adjusts owners balances
                ✓ adjusts owners tokens by index
              when sent to the owner
                ✓ keeps ownership of the token
                ✓ clears the approval for the token ID
                ✓ emits only a transfer event
                ✓ keeps the owner balance
                ✓ keeps same tokens by index
              when the address of the previous owner is incorrect
                ✓ reverts
              when the sender is not authorized for the token id
                ✓ reverts
              when the given token ID does not exist
                ✓ reverts
              when the address to transfer the token to is the zero address
                ✓ reverts
              with an invalid token id
                ✓ reverts
          to a receiver contract returning unexpected value
            ✓ reverts (41ms)
          to a receiver contract that throws
            ✓ reverts
          to a contract that does not implement the required function
            ✓ reverts
      safe mint
        via safeMint
          ✓ calls onERC721Received — without data
          to a receiver contract returning unexpected value
            ✓ reverts (39ms)
          to a receiver contract that throws
            ✓ reverts
          to a contract that does not implement the required function
            ✓ reverts
      approve
        when clearing approval
          when there was no prior approval
            ✓ clears approval for the token
            ✓ emits an approval event
          when there was a prior approval
            ✓ clears approval for the token
            ✓ emits an approval event
        when approving a non-zero address
          when there was no prior approval
            ✓ sets the approval for the target address
            ✓ emits an approval event
          when there was a prior approval to the same address
            ✓ sets the approval for the target address
            ✓ emits an approval event
          when there was a prior approval to a different address
            ✓ sets the approval for the target address
            ✓ emits an approval event
        when the address that receives the approval is the owner
          ✓ reverts
        when the sender does not own the given token ID
          ✓ reverts
        when the sender is approved for the given token ID
          ✓ reverts
        when the sender is an operator
          ✓ sets the approval for the target address
          ✓ emits an approval event
        when the given token ID does not exist
          ✓ reverts
      setApprovalForAll
        when the operator willing to approve is not the owner
          when there is no operator approval set by the sender
            ✓ approves the operator
            ✓ emits an approval event
          when the operator was set as not approved
            ✓ approves the operator
            ✓ emits an approval event
            ✓ can unset the operator approval
          when the operator was already approved
            ✓ keeps the approval to the given address
            ✓ emits an approval event
        when the operator is the owner
          ✓ reverts
      getApproved
        when token is not minted
          ✓ reverts
        when token has been minted 
          ✓ should return the zero address
          when account has been approved
            ✓ returns approved account
      totalSupply
        ✓ returns total token supply
      tokenOfOwnerByIndex
        when the given index is lower than the amount of tokens owned by the given address
          ✓ returns the token ID placed at the given index
        when the index is greater than or equal to the total tokens owned by the given address
          ✓ reverts
        when the given address does not own any token
          ✓ reverts
        after transferring all tokens to another user
          ✓ returns correct token IDs for target
          ✓ returns empty collection for original owner
      tokenByIndex
        ✓ returns all tokens
        ✓ reverts if index is greater than supply
    _mint(address, uint256)
      ✓ reverts with a null destination address
      with minted token
        ✓ emits a Transfer event
        ✓ creates the token
        ✓ adjusts owner tokens by index
        ✓ adjusts all tokens list
    _burn
      ✓ reverts when burning a non-existent token id
      with minted tokens
        with burnt token
          ✓ emits a Transfer event
          ✓ emits an Approval event
          ✓ deletes the token
          ✓ removes that token from the token list of the owner
          ✓ adjusts all tokens list
          ✓ burns all tokens
          ✓ reverts when burning a token id that has been deleted

  Contract: Core ERC721 tests for DigitalaxGarmentNFT
    Reverts
      Minting
        ✓ When sender does not have a MINTER or SMART_CONTRACT role
        ✓ When token URI is empty
        ✓ When designer is address ZERO
      Admin function
        ✓ When sender does not have a DEFAULT_ADMIN_ROLE role or SMART_CONTRACT
    Minting validation
      ✓ Correctly stored the designer
      ✓ Mints with the SMART_CONTRACT role
    Primary sale price
      ✓ Can set the primary sale price as an admin
      ✓ Can set the primary sale price as a smart contract
      ✓ Reverts when setting the primary sale price as non-admin, non-smart contract
      ✓ Reverts when setting the primary sale price for non-existent token
      ✓ Reverts when setting the primary sale price as zero
    Updating primary sale price
      ✓ Can update as admin
      ✓ Can update as smart contract
      ✓ Only records first time it happens
      ✓ Emits an event the first time it is set
      ✓ Reverts when sender is not admin or smart contract
    Updating access controls
      ✓ Can update access controls as admin
      ✓ Reverts when sender is not admin
    Updating maxChildrenPerToken
      ✓ Can update access controls as admin
      ✓ Reverts when sender is not admin
    Wrapping 1155 Child Tokens
      General
        Given a garment token that does not exist
          ✓ Returns an empty array for childContractsFor()
          ✓ Returns an empty array for childIdsForOn()
      Reverts
        When wrapping a single strand
          ✓ If referencing a token that does not exist (41ms)
          ✓ If not supplying a token reference (40ms)
        When batch wrapping multiple strands
          ✓ If referencing a token that does not exist (45ms)
          ✓ If not supplying a token reference (47ms)
      Wrapping through minting (ether from creating or minting methods)
        Single strand
          ✓ Given a garment, can mint a new strand and automatically link to the garment (78ms)
        Multiple strands
          ✓ Can link multiple strands to a garment at creation of strands (111ms)
          Given multiple strands are created
            ✓ use batchMintChildren() to link to a garment (76ms)
      Wrapping through transfers (where minting has already taken place)
        Single strand
          ✓ Successfully wraps through a transfer
        Multiple strands
          ✓ Successfully wraps through a transfer (48ms)
    burn()
      ✓ Can obtain the strands of a garment by burning (178ms)
      ✓ Can obtain a single strand of a garment by burning (112ms)
      ✓ Can burn a garment that does not have any strands (52ms)

  Contract: Core ERC721 behaviour tests for DigitalaxGenesisNFT
    Contract interface
      ERC165
        ERC165's supportsInterface(bytes4)
          ✓ uses less than 30k gas
          ✓ claims support
        supportsInterface(bytes4)
          ✓ has to be implemented
      ERC721
        ERC165's supportsInterface(bytes4)
          ✓ uses less than 30k gas
          ✓ claims support
        balanceOf(address)
          ✓ has to be implemented
        ownerOf(uint256)
          ✓ has to be implemented
        approve(address,uint256)
          ✓ has to be implemented
        getApproved(uint256)
          ✓ has to be implemented
        setApprovalForAll(address,bool)
          ✓ has to be implemented
        isApprovedForAll(address,address)
          ✓ has to be implemented
        transferFrom(address,address,uint256)
          ✓ has to be implemented
        safeTransferFrom(address,address,uint256)
          ✓ has to be implemented
        safeTransferFrom(address,address,uint256,bytes)
          ✓ has to be implemented
      ERC721Enumerable
        ERC165's supportsInterface(bytes4)
          ✓ uses less than 30k gas
          ✓ claims support
        totalSupply()
          ✓ has to be implemented
        tokenOfOwnerByIndex(address,uint256)
          ✓ has to be implemented
        tokenByIndex(uint256)
          ✓ has to be implemented
      ERC721Metadata
        ERC165's supportsInterface(bytes4)
          ✓ uses less than 30k gas
          ✓ claims support
        name()
          ✓ has to be implemented
        symbol()
          ✓ has to be implemented
        tokenURI(uint256)
          ✓ has to be implemented
    metadata
      ✓ has a name
      ✓ has a symbol
      token URI
        ✓ reverts when queried for non existent token id
        ✓ can be queried for an example token
        ✓ can be queried for all tokens
    with minted tokens
      balanceOf
        when the given address owns some tokens
          ✓ returns the amount of tokens owned by the given address
        when the given address does not own any tokens
          ✓ returns 0
        when querying the zero address
          ✓ throws
      ownerOf
        when the given token ID was tracked by this token
          ✓ returns the owner of the given token ID
        when the given token ID was not tracked by this token
          ✓ reverts
      transfers
        via transferFrom
          when called by the owner
            ✓ transfers the ownership of the given token ID to the given address
            ✓ emits a Transfer event
            ✓ clears the approval for the token ID
            ✓ emits an Approval event
            ✓ adjusts owners balances
            ✓ adjusts owners tokens by index
          when called by the approved individual
            ✓ transfers the ownership of the given token ID to the given address
            ✓ emits a Transfer event
            ✓ clears the approval for the token ID
            ✓ emits an Approval event
            ✓ adjusts owners balances
            ✓ adjusts owners tokens by index
          when called by the operator
            ✓ transfers the ownership of the given token ID to the given address
            ✓ emits a Transfer event
            ✓ clears the approval for the token ID
            ✓ emits an Approval event
            ✓ adjusts owners balances
            ✓ adjusts owners tokens by index
          when called by the owner without an approved user
            ✓ transfers the ownership of the given token ID to the given address
            ✓ emits a Transfer event
            ✓ clears the approval for the token ID
            ✓ emits an Approval event
            ✓ adjusts owners balances
            ✓ adjusts owners tokens by index
          when sent to the owner
            ✓ keeps ownership of the token
            ✓ clears the approval for the token ID
            ✓ emits only a transfer event
            ✓ keeps the owner balance
            ✓ keeps same tokens by index
          when the address of the previous owner is incorrect
            ✓ reverts
          when the sender is not authorized for the token id
            ✓ reverts
          when the given token ID does not exist
            ✓ reverts
          when the address to transfer the token to is the zero address
            ✓ reverts
        via safeTransferFrom
          with data
            to a user account
              when called by the owner
                ✓ transfers the ownership of the given token ID to the given address
                ✓ emits a Transfer event
                ✓ clears the approval for the token ID
                ✓ emits an Approval event
                ✓ adjusts owners balances
                ✓ adjusts owners tokens by index
              when called by the approved individual
                ✓ transfers the ownership of the given token ID to the given address
                ✓ emits a Transfer event
                ✓ clears the approval for the token ID
                ✓ emits an Approval event
                ✓ adjusts owners balances
                ✓ adjusts owners tokens by index
              when called by the operator
                ✓ transfers the ownership of the given token ID to the given address
                ✓ emits a Transfer event
                ✓ clears the approval for the token ID
                ✓ emits an Approval event
                ✓ adjusts owners balances
                ✓ adjusts owners tokens by index
              when called by the owner without an approved user
                ✓ transfers the ownership of the given token ID to the given address
                ✓ emits a Transfer event
                ✓ clears the approval for the token ID
                ✓ emits an Approval event
                ✓ adjusts owners balances
                ✓ adjusts owners tokens by index
              when sent to the owner
                ✓ keeps ownership of the token
                ✓ clears the approval for the token ID
                ✓ emits only a transfer event
                ✓ keeps the owner balance
                ✓ keeps same tokens by index
              when the address of the previous owner is incorrect
                ✓ reverts
              when the sender is not authorized for the token id
                ✓ reverts
              when the given token ID does not exist
                ✓ reverts
              when the address to transfer the token to is the zero address
                ✓ reverts
            to a valid receiver contract
              ✓ calls onERC721Received
              ✓ calls onERC721Received from approved
              when called by the owner
                ✓ transfers the ownership of the given token ID to the given address
                ✓ emits a Transfer event
                ✓ clears the approval for the token ID
                ✓ emits an Approval event
                ✓ adjusts owners balances
                ✓ adjusts owners tokens by index
              when called by the approved individual
                ✓ transfers the ownership of the given token ID to the given address
                ✓ emits a Transfer event
                ✓ clears the approval for the token ID
                ✓ emits an Approval event
                ✓ adjusts owners balances
                ✓ adjusts owners tokens by index
              when called by the operator
                ✓ transfers the ownership of the given token ID to the given address
                ✓ emits a Transfer event
                ✓ clears the approval for the token ID
                ✓ emits an Approval event
                ✓ adjusts owners balances
                ✓ adjusts owners tokens by index
              when called by the owner without an approved user
                ✓ transfers the ownership of the given token ID to the given address
                ✓ emits a Transfer event
                ✓ clears the approval for the token ID
                ✓ emits an Approval event
                ✓ adjusts owners balances
                ✓ adjusts owners tokens by index
              when sent to the owner
                ✓ keeps ownership of the token
                ✓ clears the approval for the token ID
                ✓ emits only a transfer event
                ✓ keeps the owner balance
                ✓ keeps same tokens by index
              when the address of the previous owner is incorrect
                ✓ reverts
              when the sender is not authorized for the token id
                ✓ reverts
              when the given token ID does not exist
                ✓ reverts
              when the address to transfer the token to is the zero address
                ✓ reverts
              with an invalid token id
                ✓ reverts
          without data
            to a user account
              when called by the owner
                ✓ transfers the ownership of the given token ID to the given address
                ✓ emits a Transfer event
                ✓ clears the approval for the token ID
                ✓ emits an Approval event
                ✓ adjusts owners balances
                ✓ adjusts owners tokens by index
              when called by the approved individual
                ✓ transfers the ownership of the given token ID to the given address
                ✓ emits a Transfer event
                ✓ clears the approval for the token ID
                ✓ emits an Approval event
                ✓ adjusts owners balances
                ✓ adjusts owners tokens by index
              when called by the operator
                ✓ transfers the ownership of the given token ID to the given address
                ✓ emits a Transfer event
                ✓ clears the approval for the token ID
                ✓ emits an Approval event
                ✓ adjusts owners balances
                ✓ adjusts owners tokens by index
              when called by the owner without an approved user
                ✓ transfers the ownership of the given token ID to the given address
                ✓ emits a Transfer event
                ✓ clears the approval for the token ID
                ✓ emits an Approval event
                ✓ adjusts owners balances
                ✓ adjusts owners tokens by index
              when sent to the owner
                ✓ keeps ownership of the token
                ✓ clears the approval for the token ID
                ✓ emits only a transfer event
                ✓ keeps the owner balance
                ✓ keeps same tokens by index
              when the address of the previous owner is incorrect
                ✓ reverts
              when the sender is not authorized for the token id
                ✓ reverts
              when the given token ID does not exist
                ✓ reverts
              when the address to transfer the token to is the zero address
                ✓ reverts
            to a valid receiver contract
              ✓ calls onERC721Received
              ✓ calls onERC721Received from approved
              when called by the owner
                ✓ transfers the ownership of the given token ID to the given address
                ✓ emits a Transfer event
                ✓ clears the approval for the token ID
                ✓ emits an Approval event
                ✓ adjusts owners balances
                ✓ adjusts owners tokens by index
              when called by the approved individual
                ✓ transfers the ownership of the given token ID to the given address
                ✓ emits a Transfer event
                ✓ clears the approval for the token ID
                ✓ emits an Approval event
                ✓ adjusts owners balances
                ✓ adjusts owners tokens by index
              when called by the operator
                ✓ transfers the ownership of the given token ID to the given address
                ✓ emits a Transfer event
                ✓ clears the approval for the token ID
                ✓ emits an Approval event
                ✓ adjusts owners balances
                ✓ adjusts owners tokens by index
              when called by the owner without an approved user
                ✓ transfers the ownership of the given token ID to the given address
                ✓ emits a Transfer event
                ✓ clears the approval for the token ID
                ✓ emits an Approval event
                ✓ adjusts owners balances
                ✓ adjusts owners tokens by index
              when sent to the owner
                ✓ keeps ownership of the token
                ✓ clears the approval for the token ID
                ✓ emits only a transfer event
                ✓ keeps the owner balance
                ✓ keeps same tokens by index
              when the address of the previous owner is incorrect
                ✓ reverts
              when the sender is not authorized for the token id
                ✓ reverts
              when the given token ID does not exist
                ✓ reverts
              when the address to transfer the token to is the zero address
                ✓ reverts
              with an invalid token id
                ✓ reverts
          to a receiver contract returning unexpected value
            ✓ reverts (46ms)
          to a receiver contract that throws
            ✓ reverts (41ms)
          to a contract that does not implement the required function
            ✓ reverts (39ms)
      approve
        when clearing approval
          when there was no prior approval
            ✓ clears approval for the token
            ✓ emits an approval event
          when there was a prior approval
            ✓ clears approval for the token
            ✓ emits an approval event
        when approving a non-zero address
          when there was no prior approval
            ✓ sets the approval for the target address
            ✓ emits an approval event
          when there was a prior approval to the same address
            ✓ sets the approval for the target address
            ✓ emits an approval event
          when there was a prior approval to a different address
            ✓ sets the approval for the target address
            ✓ emits an approval event
        when the address that receives the approval is the owner
          ✓ reverts
        when the sender does not own the given token ID
          ✓ reverts
        when the sender is approved for the given token ID
          ✓ reverts
        when the sender is an operator
          ✓ sets the approval for the target address
          ✓ emits an approval event
        when the given token ID does not exist
          ✓ reverts
      setApprovalForAll
        when the operator willing to approve is not the owner
          when there is no operator approval set by the sender
            ✓ approves the operator
            ✓ emits an approval event
          when the operator was set as not approved
            ✓ approves the operator
            ✓ emits an approval event
            ✓ can unset the operator approval
          when the operator was already approved
            ✓ keeps the approval to the given address
            ✓ emits an approval event
        when the operator is the owner
          ✓ reverts
      getApproved
        when token is not minted
          ✓ reverts
        when token has been minted 
          ✓ should return the zero address
          when account has been approved
            ✓ returns approved account
      totalSupply
        ✓ returns total token supply
      tokenOfOwnerByIndex
        when the given index is lower than the amount of tokens owned by the given address
          ✓ returns the token ID placed at the given index
        when the index is greater than or equal to the total tokens owned by the given address
          ✓ reverts
        when the given address does not own any token
          ✓ reverts
        after transferring all tokens to another user
          ✓ returns correct token IDs for target
          ✓ returns empty collection for original owner
      tokenByIndex
        ✓ returns all tokens
        ✓ reverts if index is greater than supply
    _mint(address, uint256)
      with minted token
        ✓ emits a Transfer event
        ✓ creates the token
        ✓ adjusts owner tokens by index
        ✓ adjusts all tokens list

  Contract: Core NFT tests for DigitalaxGenesisNFT
    Transfers
      ✓ Reverts when trying to transfer before the end of the genesis
    buy()
      ✓ Successfully buys a genesis NFT
      ✓ cannot buy() more than max total genesis contribution NFTs (68ms)
      ✓ total contributions are correct for multiple buyers
      ✓ Reverts when trying to contribute more than the maximum permitted
      ✓ Reverts when transfer to multisig fails (73ms)
      ✓ Reverts when the buyer has already purchased a genesis NFT
      ✓ Reverts when the contribution is lower than the minimum contribution amount (48ms)
      ✓ Reverts when attempting to buy a genesis outside of the genesis window
    adminBuy()
      ✓ Can successfully buy a genesis as an admin without contribution (51ms)
      ✓ Reverts when caller is not admin
      ✓ Reverts when beneficiary is address zero
      ✓ Reverts when beneficiary already owns a genesis NFT
    increaseContribution()
      ✓ Successfully increases a previous contribution after buy() (41ms)
      ✓ Successfully increases the total contribution after a buy and a previous contribution (65ms)
      ✓ Reverts when trying to contribute more than the maximum permitted
      ✓ Reverts when transfer to multisig fails (87ms)
      ✓ Reverts when the sender does not own a genesis
      ✓ Reverts when trying to increase a contribution outside of the genesis window
    buyOrIncreaseContribution()
      ✓ Buys a genesis when the user does not own one (48ms)
      ✓ Increases contribution when user already owns a Genesis
    updateGenesisEnd()
      ✓ Successfully updates end
      ✓ Reverts when not admin
      ✓ Reverts when being changed for a second time
      ✓ Reverts when end time already passed and trying to reopen
    updateAccessControls()
      ✓ Successfully updates access controls as admin
      ✓ Reverts when sender is not admin
      ✓ Reverts when updating to address zero

  Contract: DigitalaxMaterials 1155 behaviour tests
    createChild()
      ✓ Reverts when sender is not smart contract
      ✓ Reverts when empty uri specified
    batchCreateChildren()
      ✓ Reverts when sender is not smart contract
      ✓ Reverts when arrays are empty
      ✓ Reverts when any elem in the _uris is empty
    mintChild()
      ✓ Can successfully mint
      ✓ Reverts when sender does not have smart contract role
      ✓ Reverts when strand has not been created
      ✓ Reverts when amount is specified as zero
    batchMintChildren()
      ✓ Reverts when sender is not a smart contract
      ✓ Reverts when any strand does not exist
      ✓ Reverts when any amount is zero
      ✓ Reverts when array lengths are inconsistent
      ✓ Reverts when arrays are empty
    updateAccessControls()
      ✓ Successfully updates access controls as admin
      ✓ Reverts when sender does not have admin role
      ✓ Reverts when setting access controls to ZERO address

  Contract: DigitalaxMaterials 1155 behaviour tests
    like an ERC1155
      balanceOf
        ✓ reverts when queried about the zero address
        when accounts don't own tokens
          ✓ returns zero for given addresses
        when accounts own some tokens
          ✓ returns the amount of tokens owned by the given addresses
      balanceOfBatch
        ✓ reverts when input arrays don't match up
        ✓ reverts when one of the addresses is the zero address
        when accounts don't own tokens
          ✓ returns zeros for each account
        when accounts own some tokens
          ✓ returns amounts owned by each account in order passed
          ✓ returns multiple times the balance of the same address when asked
      setApprovalForAll
        ✓ sets approval status which can be queried via isApprovedForAll
        ✓ emits an ApprovalForAll log
        ✓ can unset approval for an operator
        ✓ reverts if attempting to approve self as an operator
      safeTransferFrom
        ✓ reverts when transferring more than balance
        ✓ reverts when transferring to zero address
        when called by the multiTokenHolder
          ✓ debits transferred balance from sender
          ✓ credits transferred balance to receiver
          ✓ emits a TransferSingle log
          ✓ preserves existing balances which are not transferred by multiTokenHolder
        when called by an operator on behalf of the multiTokenHolder
          when operator is not approved by multiTokenHolder
            ✓ reverts
          when operator is approved by multiTokenHolder
            ✓ debits transferred balance from sender
            ✓ credits transferred balance to receiver
            ✓ emits a TransferSingle log
            ✓ preserves operator's balances not involved in the transfer
        when sending to a valid receiver
          without data
            ✓ debits transferred balance from sender
            ✓ credits transferred balance to receiver
            ✓ emits a TransferSingle log
            ✓ calls onERC1155Received
          with data
            ✓ debits transferred balance from sender
            ✓ credits transferred balance to receiver
            ✓ emits a TransferSingle log
            ✓ calls onERC1155Received
        to a receiver contract returning unexpected value
          ✓ reverts
        to a receiver contract that reverts
          ✓ reverts
        to a contract that does not implement the required function
          ✓ reverts
      safeBatchTransferFrom
        ✓ reverts when transferring amount more than any of balances
        ✓ reverts when ids array length doesn't match amounts array length
        ✓ reverts when transferring to zero address
        when called by the multiTokenHolder
          ✓ debits transferred balances from sender
          ✓ credits transferred balances to receiver
          ✓ emits a TransferBatch log
        when called by an operator on behalf of the multiTokenHolder
          when operator is not approved by multiTokenHolder
            ✓ reverts
          when operator is approved by multiTokenHolder
            ✓ debits transferred balances from sender
            ✓ credits transferred balances to receiver
            ✓ emits a TransferBatch log
            ✓ preserves operator's balances not involved in the transfer
        when sending to a valid receiver
          without data
            ✓ debits transferred balances from sender
            ✓ credits transferred balances to receiver
            ✓ emits a TransferBatch log
            ✓ calls onERC1155BatchReceived
          with data
            ✓ debits transferred balances from sender
            ✓ credits transferred balances to receiver
            ✓ emits a TransferBatch log
            ✓ calls onERC1155Received
        to a receiver contract returning unexpected value
          ✓ reverts (53ms)
        to a receiver contract that reverts
          ✓ reverts (38ms)
        to a receiver contract that reverts only on single transfers
          ✓ debits transferred balances from sender
          ✓ credits transferred balances to receiver
          ✓ emits a TransferBatch log
          ✓ calls onERC1155BatchReceived
        to a contract that does not implement the required function
          ✓ reverts
      Contract interface
        ERC165
          ERC165's supportsInterface(bytes4)
            ✓ uses less than 30k gas
            ✓ claims support
          supportsInterface(bytes4)
            ✓ has to be implemented
        ERC1155
          ERC165's supportsInterface(bytes4)
            ✓ uses less than 30k gas
            ✓ claims support
          balanceOf(address,uint256)
            ✓ has to be implemented
          balanceOfBatch(address[],uint256[])
            ✓ has to be implemented
          setApprovalForAll(address,bool)
            ✓ has to be implemented
          isApprovedForAll(address,address)
            ✓ has to be implemented
          safeTransferFrom(address,address,uint256,uint256,bytes)
            ✓ has to be implemented
          safeBatchTransferFrom(address,address,uint256[],uint256[],bytes)
            ✓ has to be implemented
    internal functions
      _mint
        ✓ reverts with a zero destination address
        with minted tokens
          ✓ emits a TransferSingle event
          ✓ credits the minted amount of tokens
      _mintBatch
        ✓ reverts with a zero destination address
        ✓ reverts if length of inputs do not match
        with minted batch of tokens
          ✓ emits a TransferBatch event
          ✓ credits the minted batch of tokens
      _burn
        ✓ reverts when burning the zero account's tokens
        ✓ reverts when burning a non-existent token id
        ✓ reverts when burning more than available tokens
        with minted-then-burnt tokens
          ✓ emits a TransferSingle event
          ✓ accounts for both minting and burning
      _burnBatch
        ✓ reverts when burning the zero account's tokens
        ✓ reverts if length of inputs do not match
        ✓ reverts when burning a non-existent token id
        with minted-then-burnt tokens
          ✓ emits a TransferBatch event
          ✓ accounts for both minting and burning
    ERC1155MetadataURI
      ✓ sets the first token URI correctly
      ✓ sets the first and second token URI correctly

  Contract: MockVault tests
    ✓ Can mint an asset back strand and retrieve back the asset (107ms)
    ✓ Can mint an asset-backed strand direct to garment (76ms)


  796 passing (2m)

Done in 147.17s.
```