### osint_echoes_in_the_stone

We got a file named `__MACOSX` and a png file.

![image](https://hackmd.io/_uploads/rycvVMo3yx.png)

![image](https://hackmd.io/_uploads/H16_VfsnJe.png)

So its a normal png. And theres no hidden file as well.
Now since this challenge is osint, i reverse searched the image.
![image](https://hackmd.io/_uploads/S1NnVGo21x.png)

Weird right, so there has to be something to do with `__MACOSX` directory.
Similar past challenges had mentioned that its just AppleDoubleFormat and has nothing to do with challenge. Since the challenge was osint, I began looking up. I was finding nothing, so went back to the problem description.
> In the twilight archives of Eldoria, Nyla studies an image of an ancient Celtic cross. Her enchanted crystals illuminate the intricate carvings as she searches through forgotten tomes. The cross stands at a crossroads of realms and time, its weathered surface telling tales older than Eldoria itself. As her magical threads of knowledge connect across centuries, the true name of the monument emerges in glowing script before her eyes. Another mystery solved by the realm's most skilled information seeker, who knows that even stone can speak to those who know how to listen.

"searches through forgotten tomes" -> do I have to look in streetview of that area and look on tombs?
"stone can speak to those who listen" -> is there an online account that has posted some music?

I went to the exact location in [maps](https://maps.app.goo.gl/LYUnayruQrydq5rm9). But nothing obviously visual was seen. So, I began digging up history of `Muiredach's High Cross`. A khan academy [video](https://www.khanacademy.org/humanities/medieval-world/early-medieval-art/x4b0eb531:ireland/v/the-muiredach-cross) on this topic !!
Bruh a teammate solved it, the flag is just `HTB{Muiredach_High_Cross}` L from the side of htb team for not specifying the format they want the flag in.
###  osint_poisoned_scroll
> In her crystal-lit sanctum, Nyla examines reports of a series of magical attacks against the ruling council of Germinia, Eldoria's eastern ally. The attacks all bear the signature of the Shadow Ravens, a notorious cabal of dark mages known for their espionage across the realms. Her fingers trace connections between affected scrolls and contaminated artifacts, seeking the specific enchantment weapon deployed against the Germinian leaders. The runes along her sleeves pulse rhythmically as she sifts through intercepted messages and magical residue analyses from the attack sites. Her network of information crystals glows brighter as patterns emerge in the magical attacks—each victim touched by the same corrupting spell, though disguised under different manifestations. Finally, the name of the specific dark enchantment materializes in glowing script above her central crystal. Another dangerous threat identified by Eldoria's master information seeker, who knows that even the most sophisticated magical weapons leave distinctive traces for those who know how to read the patterns of corruption.
Poisoned Scroll: HTB{MalwareName}
Example: HTB{DarkPhantom} No special characters

So its a hacker group, that works for political espionage. And created a malware.
I first search up for such group, the group StealFalcon came into my attention as `shadow raven` has been hinted at. StealFalcon had a Project Raven. I digged on it but nothing found.
Next, I went back to description. Maybe the challenge is talking about Germany`Germania`. On searching "german politicians malware attack", the first [link](https://www.dw.com/en/russian-hackers-targeting-german-politicians-report/a-68648816), talked about a malware named WineLoader. I tried putting it in and that turned out to be the right flag.
`HTB{WineLoader}`

### osint_the_hillside_haven
We were given an image.
>Nyla stands before her largest crystal, hands weaving intricate patterns as she conjures an aerial view of the Western Hills district. A noble family's ancestral home must be located precisely—its entrance marked with a numerical rune that could unlock valuable diplomatic secrets. The crystalline vision floats above her palms, revealing winding roads and nestled dwellings along the hillsides. Her enchanted sight zooms closer as she traces the hidden pathways between estates. The magical markers on her map pulse brighter as she narrows her search, until finally, the numerical sigil above one particular doorway glows with confirmation. Another secret revealed by Eldoria's master information seeker, who knows that even among a thousand similar dwellings, each bears a unique magical signature for those with eyes to see. 


The car in the image is Honda Fit RS. The image is not from Asia. 
Perplexity did a good job at boiling down the location to `356 Longfellow Road` but that was not the flag so I went to maps.

![image](https://hackmd.io/_uploads/SkTJjRi3yx.png)

This trash can is the same we can see in the image given to us.
But the number plate of the car looks like california. And since a trash could be similar anywhere, I went with california. A teammate solved this challenge using a brute force tool(overpass turbo). Now we know a new tool exists. It was damn there in oakland 
[location](https://www.google.com/maps/@37.9006069,-122.2862508,3a,75y,172.24h,81.14t/data=!3m7!1e1!3m5!1smaZeTFX8eEmm7ouW-Gyy4A!2e0!6shttps:%2F%2Fstreetviewpixels-pa.googleapis.com%2Fv1%2Fthumbnail%3Fcb_client%3Dmaps_sv.tactile%26w%3D900%26h%3D600%26pitch%3D8.857452106444015%26panoid%3DmaZeTFX8eEmm7ouW-Gyy4A%26yaw%3D172.24226960518027!7i16384!8i8192?entry=ttu&g_ep=EgoyMDI1MDMyMy4wIKXMDSoJLDEwMjExNDU1SAFQAw%3D%3D)
