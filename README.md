VanitySearch-Bitcrack with Optimization for BTC Puzzle
Ported to CUDA 13.2 and built for the Blackwell (sm_120) architecture.

Features
<ul> <li>Optimized CUDA modular math for better performance (6900 MKeys/s on 4090, 8800 MKeys/s on 5090).</li> <li>Less RAM usage.</li> <li>Starting key setting function optimized with ECC addition and batch modular inverse.</li> <li>Easier definition of the range to scan by defining it as a power of 2.</li> <li>Only 1 GPU allowed for better efficiency.</li> <li>Only compressed addresses and prefixes.</li> <li>Pressing <b>p</b> pauses VanitySearch freeing the GPU, press <b>p</b> again to resume.</li> <li>Added prefix search. Be careful with the <code>-m</code> parameter.</li> <li><b>Random mode</b>: each GPU thread scans 1024 consecutive random keys at each step.</li> <li><b>Backup mode</b>: approximately every 60 seconds an automatic backup file is created for each GPU, containing the progress of the last sequential search. Using <code>-backup</code>, you can resume the sequential search from the last session. Useful if the program closes for any reason.</li> </ul>
Usage
text
VanitySearch [-v] [-gpuId N] [-i inputfile] [-o outputfile]
             [-start HEX] [-range BITS] [-m N] [-stop]
             [-random] [-backup] [address/prefix ...]
-v: Print version

-i inputfile: Get list of addresses/prefixes to search from specified file

-o outputfile: Output results to the specified file

-gpuId N: GPU to use, default is 0

-start HEX: Start Private Key HEX

-range BITS: Bit range dimension. start -> (start + 2^range)

-m N: Max number of prefixes found by each kernel call, default is 262144 (use multiples of 65536)

-stop: Stop when all prefixes are found

-random: Random mode active. Each GPU thread scans 1024 random sequential keys at each step. Not active by default

-backup: Backup mode allows resuming from the progress percentage of the last sequential search. Does not work with random mode.

If you want to search for multiple addresses or prefixes, insert them into the input file.

Be careful: if you are looking for multiple prefixes, it may be necessary to increase MaxFound using -m. Use multiples of 65536. The speed might decrease slightly.

In Random mode each thread selects a random number within its subrange and scans 512 keys forward and 512 keys backward. Random mode has no memory; the higher the percentage of the range that is scanned, the greater the probability that already scanned keys will be scanned again.

Donations are always welcome! :) bc1qag46ashuyatndd05s0aqeq9d6495c29fjezj09

Examples
Windows:

text
./VanitySearch.exe -gpuId 0 -i input.txt -o output.txt -start 3BA89530000000000 -range 40
text
./VanitySearch.exe -gpuId 1 -o output.txt -start 3BA89530000000000 -range 42 1MVDYgVaSN6iKKEsbzRUAYFrYJadLYZvvZ
text
./VanitySearch.exe -gpuId 0 -start 3BA89530000000000 -range 41 1MVDYgVaSN6iKKEsbzRUAYFrYJadLYZvvZ
text
./VanitySearch.exe -gpuId 0 -start 100000000000000000 -range 68 -random 19vkiEajfhuZ8bs8Zu2jgmC6oqZbWqhxhG
text
./VanitySearch.exe -gpuId 0 -start 3BA89530000000000 -range 41 -backup 1MVDYgVaSN6iKKEsbzRUAYFrYJadLYZvvZ
Linux:

text
./vanitysearch -gpuId 0 -i input.txt -o output.txt -start 3BA89530000000000 -range 40
License
VanitySearch is licensed under GPLv3.
