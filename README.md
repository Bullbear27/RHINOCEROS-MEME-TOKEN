# RHINOCEROS-MEME-TOKEN
RHINOCEROS (RHINO)Coin Is a Basically Meme Category Token on TOTAL OPEN NETWORK BLOCKCHAIN (TON). ( Ticker: RHINO )
```
use anchor_lang::prelude::*;
use anchor_spl::governance::{Governance, GovernanceAccountType};
use spl_governance::state::{GovernanceAccount, TokenGovernanceAccount};

declare_id!("Fg6PaFpoGXkYsidMpWmS3Y4R7XK6x3WjX4VQw1k");

#[program]
pub mod rhinoceros_token {
    use super::*;

    pub fn initialize(ctx: Context<Initialize>) -> ProgramResult {
        let governance_account = &mut ctx.accounts.governance_account;
        governance_account.token_governance = TokenGovernanceAccount {
            token_account: ctx.accounts.token_account.key(),
            governance_type: GovernanceAccountType::TokenGovernance,
            authority: ctx.accounts.authority.key(),
            account_type: GovernanceAccountType::Governance,
        };

        let token_account = &mut ctx.accounts.token_account;
        token_account.mint = ctx.accounts.mint.key();
        token_account.authority = ctx.accounts.authority.key();
        token_account.supply = 1122233344411;
        token_account.decimals = 0;

        Ok(())
    }

    pub fn update_social_media(ctx: Context<UpdateSocialMedia>) -> ProgramResult {
        let governance_account = &mut ctx.accounts.governance_account;
        governance_account.social_media = ctx.accounts.social_media.clone();

        Ok(())
    }

    pub fn update_token_picture(ctx: Context<UpdateTokenPicture>) -> ProgramResult {
        let governance_account = &mut ctx.accounts.governance_account;
        governance_account.token_picture = ctx.accounts.token_picture.clone();

        Ok(())
    }

    pub fn block_wallet(ctx: Context<BlockWallet>) -> ProgramResult {
        let governance_account = &mut ctx.accounts.governance_account;
        governance_account.blocked_wallets.push(ctx.accounts.wallet_to_block.key());

        Ok(())
    }

    pub fn unblock_wallet(ctx: Context<UnblockWallet>) -> ProgramResult {
        let governance_account = &mut ctx.accounts.governance_account;
        let index = governance_account
            .blocked_wallets
            .iter()
            .position(|x| *x == ctx.accounts.wallet_to_unblock.key())
            .ok_or(ProgramError::InvalidArgument)?;

        governance_account.blocked_wallets.remove(index);

        Ok(())
    }
}

#[derive(Accounts)]
pub struct Initialize<'info> {
    #[account(init, payer = authority, space = 9000)]
    pub governance_account: Account<'info, GovernanceAccount>,
    #[account(init, payer = authority, space = 100)]
    pub token_account: Account<'info, TokenAccount>,
    pub mint: Account<'info, Mint>,
    pub authority: Signer<'info>,
    pub system_program: Program<'info, System>,
}

#[derive(Accounts)]
pub struct UpdateSocialMedia<'info> {
    #[account(mut)]
    pub governance_account: Account<'info, GovernanceAccount>,
    pub authority: Signer<'info>,
    pub social_media: String,
}

#[derive(Accounts)]
pub struct UpdateTokenPicture<'info> {
    #[account(mut)]
    pub governance_account: Account<'info, GovernanceAccount>,
    pub authority: Signer<'info>,
    pub token_picture: String,
}

#[derive(Accounts)]
pub struct BlockWallet<'info> {
    #[account(mut)]
    pub governance_account: Account<'info, GovernanceAccount>,
    pub authority: Signer<'info>,
    pub wallet_to_block: AccountInfo<'info>,
}

#[derive(Accounts)]
pub struct UnblockWallet<'info> {
    #[account(mut)]
    pub governance_account: Account<'info, GovernanceAccount>,
    pub authority: Signer<'info>,
    pub wallet_to_unblock: AccountInfo<'info>,
}

#[account]
pub struct GovernanceAccount {
    pub token_governance: TokenGovernanceAccount,
    pub social_media: String,
    pub token_picture: String,
    pub blocked_wallets: Vec<Pubkey>,
}

#[account]
pub struct TokenAccount {
    pub mint: Pubkey,
    pub authority: Pubkey,
    pub supply: u64,
    pub decimals: u8,
}
```

- `initialize`:
- `update_social_media`: 
- `update_token_picture`:
- `block_wallet`: 
- `unblock_wallet`: R![1000159853](https://github.com/user-attachments/assets/27f62e2a-24a2-466b-b075-25442e29e1ce)
