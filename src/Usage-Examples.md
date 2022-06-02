<details>
<summary>To just encrypt a file</summary>
<br>
<code>dexios -e test.txt test.enc</code>
</details>
<br>
<details>
<summary>To just decrypt a file</summary>
<br>
<code>dexios -d test.enc test.txt</code>
</details>
<br>
<details>
<summary>To just erase a file</summary>
<br>
<code>dexios erase test.txt</code>
</details>
<br>
<details>
<summary>To list all possible AEADs</summary>
<br>
<code>dexios list aead</code>
</details>
<br>
<details>
<summary>To encrypt a file using <code>AES-256-GCM</code> (note: you do not need to specify <code>-a</code> when decrypting)</summary>
<br>
<code>dexios -ea2 test.txt test.enc</code>
</details>
<br>
<details>
<summary>To encrypt a file, and show the hash of the encrypted (output) file for verification later on</summary>
<br>
<code>dexios -eH test.txt test.enc</code>
</details>
<br>
<details>
<summary>To decrypt a file, and show the hash of the encrypted file (to compare with the hash generated above)</summary>
<br>
<code>dexios -dH test.enc test.txt</code>
</details>
<br>
<details>
<summary>To encrypt a file, and erase the original file</summary>
<br>
<code>dexios -e --erase test.txt test.enc</code>
</details>
<br>
<details>
<summary>To use a keyfile for encryption</summary>
<br>
<code>dexios -ek keyfile test.txt test.enc</code>
</details>
<br>
<details>
<summary>To encrypt all <code>.mp4</code> files in a directory (using <code>find</code>)</summary>
<br>
Remove `-maxdepth 1` to make this run recursively
<br>
<br>
<code>find *.mp4 -type f -maxdepth 1 -exec dexios -eyk keyfile {} {}.enc \;</code>
</details>
<br>
<details>
<summary>To decrypt all <code>.mp4.enc</code> files in a directory, and remove the <code>.enc</code> suffix</summary>
<br>
<code>find . -type f -iname "*.mp4.enc" -exec sh -c 'dexios -dk keyfile "$0" "${0%.enc}"' {} \;</code>
</details>