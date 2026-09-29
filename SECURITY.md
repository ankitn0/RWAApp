# Security Review

## 1. Self-Verification Can Undermine the KYC Trust Model

**Risk Level:** High

**Finding:**  
The deployment script enables self-verification in the KYC contract by calling `setSelfVerifyEnabled(true)`. When enabled, users can call `selfVerify()` themselves rather than going through an independent KYC/validator process.

**Why it matters:**  
If self-verification is enabled in a production deployment, an unverified user may be able to mark their own identity as verified. This weakens the trust assumptions of the KYC system and can allow users to interact with functionality intended only for verified participants.

**Suggested Fix:**  
Disable self-verification in production and require verification through an authorized KYC provider or trusted validator role. If self-verification is required for local development, ensure it is explicitly restricted to development/test deployments.

---

## 2. Sensitive KYC Information Is Stored On-Chain

**Risk Level:** High

**Finding:**  
The KYC contract stores sensitive identity-related information on-chain, including government ID, proof of address, additional documents, and nationality.

**Why it matters:**  
Blockchain data is designed to be persistent and difficult to remove. Storing sensitive personal information directly on-chain can expose users to long-term privacy and data-protection risks.

**Suggested Fix:**  
Do not store raw sensitive KYC documents or personally identifiable information on-chain. Store sensitive information in a secure off-chain system and keep only a minimal reference, commitment, or privacy-preserving verification result on-chain. Access to off-chain data should be strictly controlled.
