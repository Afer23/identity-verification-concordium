### `src/lib.rs` (Simplified Code)

```rust
use concordium_std::*;

// Struct to store user information.
#[derive(Serialize, SchemaType)]
struct User {
    id: AccountAddress,
    is_verified: bool,
}

// Struct to store the state of the contract.
#[derive(Serialize, Default)]
struct State {
    users: Vec<User>,
}

// Initialize the contract.
#[init(contract = "identity_verification")]
fn contract_init<S: HasStateApi>(_ctx: &impl HasInitContext, _state: &mut State, _amount: Amount, _logger: &mut impl HasLogger) -> InitResult<()> {
    Ok(())
}

// Function to register a new user.
#[receive(contract = "identity_verification", name = "register_user", parameter = "AccountAddress")]
fn register_user<S: HasStateApi>(ctx: &impl HasReceiveContext, host: &mut impl HasHost<State, StateApiType = S>) -> ReceiveResult<()> {
    let user_id: AccountAddress = ctx.parameter_cursor().get()?;
    let new_user = User {
        id: user_id,
        is_verified: false,
    };

    // Add the new user to the state.
    host.state_mut().users.push(new_user);

    Ok(())
}

// Function to verify a user.
#[receive(contract = "identity_verification", name = "verify_user", parameter = "AccountAddress")]
fn verify_user<S: HasStateApi>(ctx: &impl HasReceiveContext, host: &mut impl HasHost<State, StateApiType = S>) -> ReceiveResult<()> {
    let user_id: AccountAddress = ctx.parameter_cursor().get()?;

    // Find the user and mark as verified.
    for user in &mut host.state_mut().users {
        if user.id == user_id {
            user.is_verified = true;
            break;
        }
    }

    Ok(())
}

// Function to check if a user is verified.
#[receive(contract = "identity_verification", name = "is_verified", parameter = "AccountAddress", return_value = "bool")]
fn is_verified<S: HasStateApi>(ctx: &impl HasReceiveContext, host: &mut impl HasHost<State, StateApiType = S>) -> ReceiveResult<bool> {
    let user_id: AccountAddress = ctx.parameter_cursor().get()?;

    // Check if the user is verified.
    for user in &host.state().users {
        if user.id == user_id {
            return Ok(user.is_verified);
        }
    }

    Ok(false)
}
