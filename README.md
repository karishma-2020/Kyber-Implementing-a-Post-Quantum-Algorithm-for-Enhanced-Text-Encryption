# Kyber-Implementing-a-Post-Quantum-Algorithm-for-Enhanced-Text-Encryption
 Implementing a Post-Quantum Algorithm for Enhanced Text Encryption
PROPOSED ARCHITECTURE
A. Datasets Utilised
    We have created two different datasets with varying volumes of textual corpora to ensure that the proposed encryption and decryption algorithm is robust in performance. The first dataset has 120 sentences, which we consider to be a small-scale dataset, whereas the second dataset contains 600 sentences, indicating a large-scale dataset. This dual dataset strategy ensures that we test feasibility across different data volumes. The text is cleaned to guarantee that each sentence is not null and has the appropriate length for encryption and decryption.

B. Proposed Algorithm
    The starting point involves key generation through Kyber which utilizes lattice-based key encapsulation mechanisms to perform this function. A receiver uses their public key (pk) for encryption but needs their secret key (sk) for decryption. Time needed for key generation determines system efficiency. A shared secret protocol securely transfers an exchanged key between the sender and receiver after concluding the key generation steps. The sender wraps the shared secret using the public key: (ct, ss) ← extKEM.EncapSecret(pk), where ct represents the ciphertext, and ss indicates the shared secret for encryption. The ciphertext is sent to the receiver, who extracts the secret using his private key: ss′ ← extKEM.DecapSecret(ct, sk). To ensure correctness, the receiver verifies that ss = ss′, allowing both parties to be aware that they are using an identical encryption key. Encapsulation time and decapsulation time are captured to measure computational efficiency. Symmetric encryption based on the AES algorithm is carried out after establishing the shared secret. The first 16 bytes of the shared secret act as the AES key: K = ss[:16]. An arbitrary initialization vector (IV) is produced for security purposes. The message MM is padded to fit the AES block size and then encrypted: CAES = extAES − CBC − Encrypt(K, IV, M). The last ciphertext CC is constructed by placing the IV at the start followed by the AES-encrypted message: C = IV ∣∣  CAES. The decryption is in the reverse order. The IV is removed from the incoming ciphertext, and the encrypted message is obtained. With the common key, AES decryption is executed: M′ = extAES − CBC − Decrypt(K, IV,  CAES). The original padding is then eliminated after decryption to resume the plaintext message.
______________________________________________
Algorithm 1: Kyber+ Hybrid Encryption and Decryption          
Input: Text Corpus (120 sentences/600 sentences)
Output: Encryption and Decryption statistics
    (pk, sk) ← KEM.GenerateKeypair()
    record key generation time
    (ct, ss) ← KEM.EncapSecret(pk)
    send ct to receiver
    ss' ← KEM.DecapSecret(ct, sk)
    record encapsulation/decapsulation time
    K ← ss[:16]
    IV ← Generate random initialization vector
    M_padded ← Pad(M) to match AES block size
    C_AES ← AES_CBC_Encrypt(K, IV, M_padded)
    C ← IV || C_AES
    extract IV from C
    extract C_AES from C
    M_padded' ← AES_CBC_Decrypt(K, IV, C_AES)
    M' ← Remove padding from M_padded'
__________________________________________________
    The Kyber+ hybrid encryption scheme ensures quantum-resistant key exchange and secure symmetric encryption, combining the robustness of lattice-based cryptography with the efficiency of AES encryption.

C. Implementation of Hybrid Encryption and Decryption Framework
    To enhance the security and randomness of Kyber we introduce hybrid encapsulation which combines Kyber and AES Symmetric encryption as shown in Figure 1 below. This approach leverages Kyber’s quantum resistance for key exchange while utilizing AES for efficient and fast symmetric encryption, ensuring both robustness and performance.

Fig. 1. Proposed Architecture.

    1) Key Generation using Kyber: Kyber is a lattice-based KEM that provides quantum-resistant key exchange. The function initiates the encryption process by generating a public-private key pair. A public key is first generated via kem.generate_keypair(), which will be used to encapsulate the shared secret. The key encapsulation module automatically handles the private key. The assessment of key generation computational efficiency happens at this phase through time-based performance evaluations.
   2) Key Encapsulation and Decapsulation: A key is first created, and then the public key becomes essential to perform key exchange and to encapsulate a shared secret. A ciphertext is generated after encryption of the shared secret with kem.encap_secret(public_key). The procedure generates ciphertext that enables safe key exchange while providing a shared secret, which will function as AES's symmetric key encryption. Through kem.decap_secret(ciphertext) the shared secret gets revealed to enable the receiver to utilize the same encryption key. Security efficiency measures are recorded through time tracking during encryption and decryption operations at this point.
   3) AES Symmetric Encryption: A shared secret becomes effective in encrypting messages using AES encryption algorithms. The first 16 bytes from the produced shared secret serve as the AES encryption key. An extra layer of protection derives from a randomly selected initialization vector (IV) which is randomly picked. The padding process adds characters to the plaintext until the amount matches the AES block size requirements. The encrypted message is produced by AES encryption in CBC mode. Security decryption through the generated ciphertext requires a prefix addition of the IV key.
   4) AES Symmetric Decryption: Symmetric decryption functions as an exact opposite process to symmetric encryption while including the IV as part of the ciphertext. The encryption algorithm contains an IV that is included in the resulting ciphertext. The shared secret is utilized to decrypt the rest of the ciphertext. The original plaintext is then retrieved by unpadding the decrypted message. The process is a secure and efficient hybrid encryption scheme that is a quantum-resistant key exchange.

D. Kyber Variants’ Implementation With Multithreading For Parallel Execution
    We utilize three variants of Kyber for boosting quantum attack protection. We use Kyber512, which is a light version with a medium level of security and lesser computational overhead. The second variant that we use is Kyber768, with balanced security and efficiency. The third and final instance is Kyber1024, which offers maximum security against quantum threats but with increasing computational complexity.

Fig. 2. Multithreading on Kyber Variants.

    To ensure that our system offers good efficiency and speed, we make use of the multithreading concept which allows us to run the encryption algorithm parallelly. Figure 2 above illustrates that all three variants of Kyber+ are processed in distinct threads which invoke the Kyber process and outputs results. Also, we make sure that every thread is initialized in parallel and has fully finished its execution before the final results are merged.

E. Metrics and Evaluation
    The security and effectiveness of the proposed encryption technique have been carefully examined by implementing the evaluation criteria listed below:
    1) Key Generation Time: The performance of key pairs generated by the different Kyber Variants is reported by computing the time difference between the start and finish of the key generation process.
   2) Encryption and Decryption Time: This is the time it takes to successfully encrypt and decrypt messages. The duration of the encryption and decryption process in our methodology is the metric for this examination.
   3) Shannon Entropy of Ciphertext: It is an indicator of randomness of ciphertext. In this case, HCT its Shannon entropy, ci is the i-th ciphertext's individual byte, and P(ci) is its probability of occurrence.
HCT =-i= 1256P(ci) * log2P(ci)

    4) Shannon Entropy on AES Encrypted Text: Assesses the level of randomness in the encrypted text. In the formula below, HEM represents the Shannon entropy of the encrypted message, and here e𝑖 stands for the i-th unique byte in the encrypted message, and 𝑃(e𝑖) reflects the probability of occurrence of e𝑖.
HEM =-i= 1256P(ei) * log2P(ei)

    5) Hamming Distance: Evaluates the bit-wise difference between two ciphertexts in order to analyze the effectiveness of encryption. The formula below mentions, HD(C1, C2) which refers to the hamming distance between ciphertexts C1 and C2, n = total number of bytes in the ciphertexts, C1[i] and C2[i] are the corresponding bytes in the two ciphertexts, ⊕ operator stands for bitwise XOR operation and Weight(X) denotes the number of 1s in the binary representation of 𝑋 (Hamming Weight).
HD(C1, C2) = i = 1n Weight(C1[i]  C2[i])
