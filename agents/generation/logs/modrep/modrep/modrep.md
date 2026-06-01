OpenAI Codex v0.133.0
--------
workdir: /Users/mac/Documents/Rethlas/agents/generation
model: gpt-5.4
provider: custom
approval: never
sandbox: danger-full-access
reasoning effort: xhigh
reasoning summaries: none
session id: 019e81de-ab00-7cb3-b0b1-125f19d8abff
--------
user
Use AGENTS.md exactly to solve the math problem in data/modrep/modrep.md. Use problem_id=modrep/modrep. Use reference_dir=data/modrep/modrep.refs if it exists.
codex
I'll start by reading the problem file and checking for reference materials, then initialize memory.
exec
/bin/zsh -lc 'cat data/modrep/modrep.md' in /Users/mac/Documents/Rethlas/agents/generation
 succeeded in 0ms:
Prove that for $\overline{\mathbb{F}}_p$-representations of a finite group $G$, the trivial representation has a finite resolution by representations induced from its Sylow p-group.

exec
/bin/zsh -lc 'ls -la data/modrep/modrep.refs/ 2>/dev/null && echo "---EXISTS---" || echo "---NO REFS DIR---"' in /Users/mac/Documents/Rethlas/agents/generation
 succeeded in 0ms:
total 8
drwxr-xr-x@ 3 mac  staff   96 May 29 17:45 .
drwxr-xr-x@ 4 mac  staff  128 May 29 17:45 ..
-rw-r--r--@ 1 mac  staff  366 May 29 17:45 ref1.md
---EXISTS---

exec
/bin/zsh -lc 'cat data/modrep/modrep.refs/ref1.md' in /Users/mac/Documents/Rethlas/agents/generation
 succeeded in 0ms:
Modular representation studies representations of finite groups in characteristic $\ell$, i.e. $\overline{\mathbb{F}_ \ell}$-representations of a finite group. For finite group of Lie type $G(\mathbb{F}_ p)$, where $G$ is a split reductive group defined over $\mathbb{F}_ p$ there are two cases:

1. $p\neq \ell$.
2. $p =\ell$ (called "in defining characteristic").

exec
/bin/zsh -lc 'ls results/ 2>/dev/null; ls memory/ 2>/dev/null' in /Users/mac/Documents/Rethlas/agents/generation
 succeeded in 0ms:
example
sum
example
sum

codex
Now let me create the results directory and initialize memory for this problem.
exec
/bin/zsh -lc 'mkdir -p results/modrep/modrep memory/modrep/modrep' in /Users/mac/Documents/Rethlas/agents/generation
 succeeded in 0ms:
codex
Good. Let me now understand the problem and initialize memory.

The problem: Prove that for $\overline{\mathbb{F}}_p$-representations of a finite group $G$, the trivial representation has a finite resolution by representations induced from its Sylow $p$-group.

There's a typo "Sylow" — it means "Sylow $p$-subgroup". The problem is about modular representation theory: we're working over $\overline{\mathbb{F}}_p$, the algebraic closure of the finite field with $p$ elements, and we need to show the trivial module has a finite resolution by modules induced from a Sylow $p$-subgroup $P$ of $G$.

Let me initialize memory.
exec
/bin/zsh -lc "cd /Users/mac/Documents/Rethlas/agents/generation && python3 -c \"
import json, os
os.makedirs('memory/modrep/modrep', exist_ok=True)
meta = {
    'problem_id': 'modrep/modrep',
    'problem_statement': 'Prove that for \\overline{\\\\mathbb{F}}_p-representations of a finite group G, the trivial representation has a finite resolution by representations induced from its Sylow p-subgroup.',
    'reference_dir': 'data/modrep/modrep.refs',
    'references': ['ref1.md']
}
with open('memory/modrep/modrep/meta.json', 'w') as f:
    json.dump(meta, f, indent=2)
print('Memory initialized')
\"" in /Users/mac/Documents/Rethlas/agents/generation
 succeeded in 0ms:
<string>:6: SyntaxWarning: "\o" is an invalid escape sequence. Such sequences will not work in the future. Did you mean "\\o"? A raw string is also an option.
Memory initialized

  → Analyze problem and obtain immediate conclusions
  • Search for relevant modular representation theory results
  • Construct examples and counterexamples
  • Propose decomposition plans and prove
  • Write blueprint and verify proof
codex
Now let me think about the problem. This is about modular representation theory over characteristic $p$. Let me parse it carefully:

- $G$ is a finite group
- We work with $\overline{\mathbb{F}}_p$-representations (representations over the algebraic closure of $\mathbb{F}_p$)
- $P$ is a Sylow $p$-subgroup of $G$
- $\mathbf{1}$ (or $\overline{\mathbb{F}}_p$) is the trivial representation
- We need to show $\mathbf{1}$ has a finite resolution by representations induced from $P$

A "resolution" means an exact sequence:
$$0 \to M_n \to \cdots \to M_1 \to M_0 \to \mathbf{1} \to 0$$
where each $M_i$ is a direct sum of modules of the form $\operatorname{Ind}_P^G(V)$ for some $\overline{\mathbb{F}}_P$-module $V$.

Let me invoke the `obtain-immediate-conclusions` skill first.
exec
/bin/zsh -lc "cat /Users/mac/.codex/skills/.system/obtain-immediate-conclusions/SKILL.md 2>/dev/null || find /Users/mac/.codex/skills -name \"SKILL.md\" -path \"*obtain-immediate*\" -exec cat {} \\;" in /Users/mac/Documents/Rethlas/agents/generation
 succeeded in 0ms:
exec
/bin/zsh -lc 'find /Users/mac -maxdepth 5 -name "SKILL.md" -path "*obtain-immediate*" 2>/dev/null' in /Users/mac/Documents/Rethlas/agents/generation
 succeeded in 4949ms:
codex
The skill file appears empty. Let me proceed with reasoning directly based on the AGENTS.md instructions and my mathematical knowledge.

Let me think about what immediate conclusions we can draw:

**Understanding the problem**: Let $G$ be a finite group, $p$ a prime, $P \in \text{Syl}_p(G)$. Consider $\overline{\mathbb{F}}_p[G]$-modules. We want a finite resolution of the trivial module $\overline{\mathbb{F}}_p$ by modules induced from $P$.

**Key immediate observations:**

1. **The trivial module is the one-dimensional representation where every $g \in G$ acts as identity.** Over $\overline{\mathbb{F}}_p$, $\operatorname{Ind}_P^G(\overline{\mathbb{F}}_p) \cong \overline{\mathbb{F}}_p[G/P]$, the permutation module on cosets.

2. **Since $P$ is a Sylow $p$-subgroup, $[G:P]$ is coprime to $p$.** This means the trivial module is a direct summand of $\operatorname{Ind}_P^G(\overline{\mathbb{F}}_p)$ via the trace map: the map $\operatorname{Ind}_P^G(\overline{\mathbb{F}}_p) \to \overline{\mathbb{F}}_p$ sending $gP \mapsto 1$ splits because $[G:P]$ is invertible in $\overline{\mathbb{F}}_p$.

3. **This is essentially the statement that the trivial module is relatively $P$-projective.** In modular representation theory, a $kG$-module is relatively $P$-projective (where $P$ is a Sylow $p$-subgroup) if and only if it is $P$-projective... but more importantly, *every* $kG$-module is relatively $P$-projective when $P$ is a Sylow $p$-subgroup (this is a fundamental result in modular representation theory).

4. **The trivial module is a direct summand of $\operatorname{Ind}_P^G(\overline{\mathbb{F}}_p)$.** The proof: the map $\overline{\mathbb{F}}_p \to \operatorname{Ind}_P^G(\overline{\mathbb{F}}_p)$ sending $1 \mapsto \sum_{g \in G/P} g\cdot 1$ is $G$-equivariant (all cosets get the same coefficient 1), and composing with the augmentation map gives multiplication by $[G:P]$, which is invertible mod $p$. So we have a split injection $\overline{\mathbb{F}}_p \hookrightarrow \operatorname{Ind}_P^G(\overline{\mathbb{F}}_p)$.

5. **A resolution of length 0?** Wait, the statement asks for a "finite resolution by representations induced from its Sylow $p$-group." This is actually trivial if we just mean that the trivial module itself can be expressed... Let me think more carefully.

Actually, I think the statement needs more precision. Let me think about what "resolution" means here:

A resolution of $\mathbf{1}$ by induced modules from $P$ would be an exact sequence:
$$\cdots \to I_2 \to I_1 \to I_0 \to \mathbf{1} \to 0$$
where each $I_i$ is a direct sum of modules of the form $\operatorname{Ind}_P^G(M)$ for $\overline{\mathbb{F}}_pP$-modules $M$.

But wait: **every** $\overline{\mathbb{F}}_pG$-module is relatively $P$-projective! This is because $p \nmid [G:P]$. In characteristic $p$, every $kG$-module is projective relative to a Sylow $p$-subgroup.

So... the trivial module $\mathbf{1}$ is relatively $P$-projective. But this doesn't directly give a resolution. The standard result is:

If $M$ is relatively $P$-projective, then $M$ is a direct summand of $\operatorname{Ind}_P^G(\operatorname{Res}_P^G(M))$. So $\mathbf{1} \mid \operatorname{Ind}_P^G(\operatorname{Res}_P^G(\mathbf{1})) = \operatorname{Ind}_P^G(\mathbf{1}_P)$.

This gives the trivial module as a direct summand, which means we have:
$$\operatorname{Ind}_P^G(\mathbf{1}_P) \cong \mathbf{1} \oplus M$$
for some module $M$.

But this isn't a "resolution." For a resolution, we want each term to be induced from $P$. If $\mathbf{1}$ is a direct summand of $\operatorname{Ind}_P^G(\mathbf{1}_P)$, then we have a split epimorphism $\operatorname{Ind}_P^G(\mathbf{1}_P) \twoheadrightarrow \mathbf{1}$. The kernel would also be a direct summand of something induced from $P$, and by induction on dimension perhaps...

Let me think about this more carefully. The standard approach for constructing a resolution of a relatively projective module by induced modules goes as follows:

Since $\operatorname{Ind}_P^G(\operatorname{Res}_P^G(\mathbf{1})) = \operatorname{Ind}_P^G(\mathbf{1}_P)$ maps onto $\mathbf{1}$ (the map comes from the relative projectivity property: the natural map $\operatorname{Ind}_P^G(\operatorname{Res}_P^G(M)) \to M$ is split surjective), we get:

$$\operatorname{Ind}_P^G(\operatorname{Res}_P^G(\mathbf{1})) \twoheadrightarrow \mathbf{1}$$

Let $K_0$ be the kernel. Then apply the same argument to $K_0$: $K_0$ is relatively $P$-projective, so $\operatorname{Ind}_P^G(\operatorname{Res}_P^G(K_0)) \twoheadrightarrow K_0$, giving:

$$\operatorname{Ind}_P^G(\operatorname{Res}_P^G(K_0)) \to \operatorname{Ind}_P^G(\operatorname{Res}_P^G(\mathbf{1})) \to \mathbf{1} \to 0$$

But wait, this doesn't terminate unless we can show the kernels eventually become zero. The dimension of the kernel would need to shrink...

Hmm, let me think differently. Actually there's a standard result:

In characteristic $p$, the trivial representation $\mathbf{1}$ has a resolution by permutation modules on $p$-subgroup cosets. This is related to the **Steinberg complex** or **Tits building** for the group.

For a general finite group $G$, the **sylow $p$-subgroup complex** (the poset of nontrivial $p$-subgroups) gives a resolution. More precisely, there's the **Webb conjecture / theorem** about the $p$-subgroup complex, or the **Brown complex**.

Actually, wait — there's a simpler approach. This problem might be about the **relative homological algebra** or about the fact that the trivial module has a finite projective resolution in the category of $\overline{\mathbb{F}}_pG$-modules (since the group algebra is finite-dimensional, hence self-injective). But the resolution is by modules *induced from $P$*, not by projectives.

Let me think about this more. The **Green correspondence** or more generally, the theory of **vertices and sources** is relevant.

Given any indecomposable $\overline{\mathbb{F}}_pG$-module $M$, its vertex is a $p$-subgroup $Q$ of $G$ (defined up to conjugacy), and $M$ is a direct summand of $\operatorname{Ind}_Q^G(N)$ for some $\overline{\mathbb{F}}_pQ$-module $N$. The trivial module $\mathbf{1}$ has vertex equal to a Sylow $p$-subgroup $P$ (since $\mathbf{1}$ is not a direct summand of anything induced from a proper subgroup of $P$, but it IS a direct summand of $\operatorname{Ind}_P^G(\mathbf{1}_P)$).

So we know $\mathbf{1} \mid \operatorname{Ind}_P^G(\mathbf{1}_P)$. But that's just one term. How do we get a full resolution?

I think the resolution comes from the **standard relative projective resolution**: for any $kG$-module $M$ that is relatively $P$-projective, we can construct a resolution:

$$\cdots \to \operatorname{Ind}_P^G(\operatorname{Res}_P^G(J \otimes \operatorname{Res}_P^G(M))) \to \operatorname{Ind}_P^G(\operatorname{Res}_P^G(M)) \to M \to 0$$

where $J$ is the augmentation ideal of $kP$, and the terms involve $\operatorname{Ind}_P^G \circ \operatorname{Res}_P^G$ repeatedly. But does this terminate? In general, no - it's infinite.

Hmm, but the problem says "finite resolution." Let me reconsider...

Wait - maybe the answer is much simpler. The claim is that the trivial module $\mathbf{1}$ has a finite resolution by modules *induced from $P$*. This is essentially asking for the **relative global dimension** or something similar.

Actually, I think I'm overcomplicating this. Let me re-read: "the trivial representation has a finite resolution by representations induced from its Sylow p-group."

In characteristic $p$, the group algebra $\overline{\mathbb{F}}_pG$ is a Frobenius algebra, and in particular, every module has a finite projective resolution (in fact, the projective dimension of any module is either 0 or $\infty$, since $kG$ is self-injective).

But the resolution is by *induced* modules from $P$, not by projective $kG$-modules. Induced modules from $P$ are $kG$-modules of the form $\operatorname{Ind}_P^G(V)$. These are *not* necessarily projective $kG$-modules (unless $P$ is trivial, which it isn't in general).

OK let me step back and think about what results I know:

1. Every $kG$-module is relatively $P$-projective (since $P$ is a Sylow $p$-subgroup).

2. The functor $\operatorname{Ind}_P^G$ is left adjoint to $\operatorname{Res}_P^G$ and is exact.

3. The trivial module $\mathbf{1}_G$ is a direct summand of $\operatorname{Ind}_P^G(\mathbf{1}_P)$.

Now, if we can show that the category of $\overline{\mathbb{F}}_pG$-modules has enough "induced-from-$P$" objects (which are relatively $P$-projective), and that the relative homological algebra has finite global dimension...

Actually, I think the resolution might come from the **$p$-local structure** of $G$. Let me think about the **normalizer of $P$** and the **Brauer quotient**.

Wait, I think there's a different approach. Let me consider the **bar resolution** for the group $G$ relative to $P$:

For a $kG$-module $M$, the bar resolution relative to $P$ is:
$$\cdots \to kG \otimes_{kP} kG \otimes_{kP} M \to kG \otimes_{kP} M \to M \to 0$$

where the differentials are defined. If we apply this to $M = \mathbf{1}$, we get:
$$\cdots \to kG \otimes_{kP} kG \otimes_{kP} k \to kG \otimes_{kP} k \to k \to 0$$

Now, is this finite? No, it's infinite in general.

Let me reconsider. Perhaps the statement is about something else entirely. Let me think about what "finite resolution by representations induced from its Sylow $p$-group" could mean.

Hmm, maybe it's about the **existence of a resolution** where each term is a *direct sum of induced modules from $P$*, and the resolution has *finite length*. 

The key insight might be: since $\mathbf{1}$ is relatively $P$-projective, and every module is relatively $P$-projective, we might be able to construct a resolution using the *relative* version of the standard projective resolution.

Actually wait - I just realized something. In the **stable module category** or in **relative homological algebra**, for a $p$-group $P$, the trivial module has a projective resolution of length 0 (since for a $p$-group, the only simple module is trivial and all modules are projective iff the group is trivial... no that's wrong).

Let me think about the **specific case where $P$ is a Sylow $p$-subgroup**. There's a classical result:

For a finite group $G$ and a Sylow $p$-subgroup $P$, the induced module $\operatorname{Ind}_P^G(k)$ (where $k$ has trivial $P$-action) contains the trivial $G$-module as a direct summand. The complementary summand is called the **Steinberg module** (in the context of finite groups of Lie type) or more generally, there is the **$p$-permutation module** structure.

Actually, I think I've been overthinking this. Let me look at this from the perspective of **Green's theory of vertices and sources** and the concept of **$P$-projective resolutions**.

Every $kG$-module $M$ is relatively $P$-projective. The standard way to show this is: the map $\operatorname{Ind}_P^G(\operatorname{Res}_P^G(M)) \to M$ given by $\sum g \otimes m_g \mapsto \sum g m_g$ is a split epimorphism, because the map $M \to \operatorname{Ind}_P^G(\operatorname{Res}_P^G(M))$ given by $m \mapsto \frac{1}{[G:P]} \sum_{g \in G/P} g \otimes g^{-1}m$ splits it (and $[G:P]^{-1}$ exists in $k$ since $\operatorname{char} k = p \nmid [G:P]$).

So for $M = \mathbf{1}$, we get a split exact sequence:
$$0 \to K \to \operatorname{Ind}_P^G(\mathbf{1}_P) \to \mathbf{1} \to 0$$

Now $K$ is also a $kG$-module, so it too is relatively $P$-projective. So we can repeat:
$$0 \to K_1 \to \operatorname{Ind}_P^G(\operatorname{Res}_P^G(K)) \to K \to 0$$

Splicing these gives:
$$0 \to K_1 \to \operatorname{Ind}_P^G(\operatorname{Res}_P^G(K)) \to \operatorname{Ind}_P^G(\mathbf{1}_P) \to \mathbf{1} \to 0$$

But this is an infinite process...

UNLESS we can show that after finitely many steps, the kernel becomes an induced module itself. For $p$-groups specifically, but we don't have a $p$-group.

Hmm wait, let me think about the dimension. $\dim \operatorname{Ind}_P^G(\mathbf{1}_P) = [G:P] \cdot \dim \mathbf{1}_P = [G:P]$. And $\dim \mathbf{1} = 1$. So $\dim K = [G:P] - 1$, which is strictly larger!

So this naive approach doesn't terminate because the resolutions are getting larger, not smaller. This can't be a resolution by projective covers.

Let me completely rethink. Maybe the resolution goes the other way? Like, maybe we want:
$$0 \to \mathbf{1} \to I_0 \to I_1 \to \cdots$$

No, that would be an injective resolution. Or maybe we want:
$$0 \to I_n \to \cdots \to I_0 \to \mathbf{1} \to 0$$
where each $I_i$ is induced from $P$, but we DON'T require the maps to come from the relative projectivity property. Instead, we construct the resolution from something like the **Tits building** or **the complex of $p$-subgroups**.

Actually, I think the answer involves the **complex of nontrivial $p$-subgroups** of $G$. This is a simplicial complex $\mathcal{S}_p(G)$ whose simplices are chains of nontrivial $p$-subgroups. The group $G$ acts by conjugation. The reduced chain complex of $\mathcal{S}_p(G)$ gives a resolution of the trivial module... but the terms are permutation modules on $G/Q$ for various $p$-subgroups $Q$. Not just induced from the Sylow $p$-subgroup.

Wait, but the problem specifically says "induced from its Sylow $p$-group" (singular). So all terms are induced from the same Sylow $p$-subgroup $P$.

That's a strong restriction. All modules in the resolution are of the form $\operatorname{Ind}_P^G(V)$ for various $kP$-modules $V$.

Hmm, actually for $k = \overline{\mathbb{F}}_p$, $kP$ is a local algebra (since $P$ is a $p$-group), so there's a unique simple $kP$-module: the trivial module. Every projective indecomposable $kP$-module is the free module $kP$ itself.

So the modules induced from $P$ that we can use are of the form $\operatorname{Ind}_P^G(kP) = kG$ (the free $kG$-module of rank 1) and direct sums of $\operatorname{Ind}_P^G(k)$ (the permutation module $k[G/P]$).

Now, $\mathbf{1}$ is a direct summand of $k[G/P]$, so we can write $k[G/P] = \mathbf{1} \oplus M$. But then what?

Actually, I think the question might be easier than I'm making it. Let me reconsider whether the answer could be:

The trivial module has a finite resolution by modules induced from the Sylow $p$-subgroup *because* we can use the standard bar resolution construction but stop after a finite number of steps by using the fact that $p \nmid [G:P]$.

Let me search for relevant results. Let me use the search-math-results skill. But first, let me record my immediate conclusions in memory.
exec
/bin/zsh -lc "cd /Users/mac/Documents/Rethlas/agents/generation && python3 -c \"
import json, time

def append_to(channel, content):
    path = f'memory/modrep/modrep/{channel}.jsonl'
    with open(path, 'a') as f:
        f.write(json.dumps({'timestamp': time.time(), 'content': content}) + '\\n')
    print(f'Appended to {channel}')

append_to('immediate_conclusions', '''
IMMEDIATE CONCLUSIONS:

1. Problem restatement: Let G be a finite group, k = \\overline{\\mathbb{F}}_p, P a Sylow p-subgroup.
   Show that the trivial kG-module k has a finite resolution:
   0 \\to I_n \\to ... \\to I_0 \\to k \\to 0
   where each I_i is a direct sum of modules of the form Ind_P"'^G(V) for kP-modules V.

2. Key facts:
   - p '"\\nmid [G:P] since P is a Sylow p-subgroup.
   - Every kG-module M is relatively P-projective: the natural map
     \\mu: Ind_P"'^G(Res_P''^G(M)) '"\\to M is split surjective.
     The splitting uses the trace map divided by [G:P].
   - In particular, k | Ind_P"'^G(k_P). Specifically, Ind_P''^G(k_P) '"\\cong k[G/P]
     (permutation module on cosets), and k is a direct summand with complement
     of dimension [G:P]-1.

3. The map: \\iota: k \\to Ind_P"'^G(k) sending 1 '"\\mapsto \\sum_{g \\in G/P} g \\otimes 1
   followed by augmentation \\epsilon: Ind_P"'^G(k) '"\\to k sending g \\otimes 1 \\mapsto 1
   composes to [G:P], which is invertible. So \\epsilon \\circ (\\iota/[G:P]) = id,
   giving a split exact sequence.

4. Since k is relatively P-projective, the relative bar resolution exists:
   ... \\to kG \\otimes_{kP} kG \\otimes_{kP} k \\to kG \\otimes_{kP} k \\to k \\to 0
   But this is infinite in general.

5. The question may be about the WEB COMPLEX or p-SUBGROUP COMPLEX:
   The complex of nontrivial p-subgroups of G gives a finite resolution of the
   trivial module whose terms are permutation modules k[G/Q] for p-subgroups Q.
   However, this uses all p-subgroups, not just the Sylow p-subgroup.

6. Need to determine: is there a specific resolution using ONLY modules induced from
   a fixed Sylow p-subgroup P? This is the key question.
''')\" 2>/dev/null" in /Users/mac/Documents/Rethlas/agents/generation
 succeeded in 0ms:
Appended to immediate_conclusions

codex
Now let me use the search-math-results skill to find relevant theorems.
exec
/bin/zsh -lc 'cat /Users/mac/.codex/skills/.system/search-math-results/SKILL.md 2>/dev/null | head -80' in /Users/mac/Documents/Rethlas/agents/generation
 succeeded in 0ms:
codex
The skill file is empty — I'll proceed with searching directly. Let me look for relevant MCP resources and do web searches.
mcp: arxiv-mcp/list_mcp_resources started
mcp: arxiv-mcp/list_mcp_resources (failed)
resources/list failed: unknown MCP server 'arxiv-mcp'
2026-06-01T06:30:47.583954Z ERROR codex_core::tools::router: error=resources/list failed: unknown MCP server 'arxiv-mcp'
mcp: codex/list_mcp_resources started
mcp: codex/list_mcp_resources (completed)
codex
No MCP resources available. Let me check what tools are available and search the web.
exec
/bin/zsh -lc 'cd /Users/mac/Documents/Rethlas/agents/generation && which python3 && python3 -c "import sys; print(sys.path[:5])"' in /Users/mac/Documents/Rethlas/agents/generation
 succeeded in 0ms:
/Users/mac/Documents/Rethlas/agents/generation/.venv/bin/python3
['', '/opt/homebrew/Cellar/python@3.14/3.14.5/Frameworks/Python.framework/Versions/3.14/lib/python314.zip', '/opt/homebrew/Cellar/python@3.14/3.14.5/Frameworks/Python.framework/Versions/3.14/lib/python3.14', '/opt/homebrew/Cellar/python@3.14/3.14.5/Frameworks/Python.framework/Versions/3.14/lib/python3.14/lib-dynload', '/Users/mac/Documents/Rethlas/agents/generation/.venv/lib/python3.14/site-packages']

codex
Let me search the web for results about resolutions of the trivial module by induced modules from Sylow subgroups.
exec
/bin/zsh -lc "cd /Users/mac/Documents/Rethlas/agents/generation && python3 -c \"
import urllib.request, json

# Search for mathematical results about trivial representation resolution by induced from Sylow
query = 'trivial representation finite resolution induced from Sylow p-subgroup modular representation'
url = f'https://api.duckduckgo.com/?q={urllib.request.quote(query)}&format=json'
try:
    req = urllib.request.Request(url, headers={'User-Agent': 'Mozilla/5.0'})
    resp = urllib.request.urlopen(req, timeout=10)
    data = json.loads(resp.read())
    print(json.dumps(data, indent=2)[:3000])
except Exception as e:
    print(f'Error: {e}')
\" 2>&1 | head -60" in /Users/mac/Documents/Rethlas/agents/generation
 succeeded in 246ms:
{
  "Abstract": "",
  "AbstractSource": "",
  "AbstractText": "",
  "AbstractURL": "",
  "Answer": "",
  "AnswerType": "",
  "Definition": "",
  "DefinitionSource": "",
  "DefinitionURL": "",
  "Entity": "",
  "Heading": "",
  "Image": "",
  "ImageHeight": "",
  "ImageIsLogo": "",
  "ImageWidth": "",
  "Infobox": "",
  "Redirect": "",
  "RelatedTopics": [],
  "Results": [],
  "Type": "",
  "meta": {
    "attribution": null,
    "blockgroup": null,
    "created_date": "2021-03-24",
    "description": "testing",
    "designer": null,
    "dev_date": "2021-03-24",
    "dev_milestone": "development",
    "developer": [
      {
        "name": "zt",
        "type": "duck.co",
        "url": "https://duck.co/user/zt"
      }
    ],
    "example_query": "",
    "id": "just_another_test",
    "is_stackexchange": 0,
    "js_callback_name": "another_test",
    "live_date": null,
    "maintainer": {
      "github": ""
    },
    "name": "Just Another Test",
    "perl_module": "DDG::Lontail::AnotherTest",
    "producer": null,
    "production_state": "offline",
    "repo": "fathead",
    "signal_from": "just_another_test",
    "src_domain": "how about there",
    "src_id": null,
    "src_name": "hi there",
    "src_options": {
      "directory": "",
      "is_fanon": 0,
      "is_mediawiki": 0,
      "is_wikipedia": 0,
      "language": "",
      "min_abstract_length": null,

codex
The DuckDuckGo API isn't returning useful results. Let me try a different approach and reason more deeply about this problem, then also search for relevant papers on arXiv.
exec
/bin/zsh -lc "cd /Users/mac/Documents/Rethlas/agents/generation && python3 -c \"
import urllib.request, json, xml.etree.ElementTree as ET

# Search arXiv API
query = 'all:trivial+AND+all:resolution+AND+all:Sylow+AND+all:induced+AND+all:module'
url = f'http://export.arxiv.org/api/query?search_query={urllib.request.quote(query)}&max_results=10'
try:
    req = urllib.request.Request(url)
    resp = urllib.request.urlopen(req, timeout=15)
    data = resp.read().decode()
    # Extract titles and summaries
    root = ET.fromstring(data)
    ns = {'atom': 'http://www.w3.org/2005/Atom'}
    entries = root.findall('atom:entry', ns)
    for i, entry in enumerate(entries[:5]):
        title = entry.find('atom:title', ns).text.strip()
        summary = entry.find('atom:summary', ns).text.strip()[:300]
        link = entry.find('atom:id', ns).text.strip()
        print(f'{i+1}. {title}')
        print(f'   ID: {link}')
        print(f'   Summary: {summary}')
        print()
except Exception as e:
    print(f'Error: {e}')
\" 2>&1" in /Users/mac/Documents/Rethlas/agents/generation
 succeeded in 2187ms:
exec
/bin/zsh -lc "cd /Users/mac/Documents/Rethlas/agents/generation && python3 -c \"
import urllib.request, json, xml.etree.ElementTree as ET

# Search arXiv API - simpler query
query = 'all:trivial+AND+all:resolution+AND+all:induced+AND+all:Sylow'
url = f'http://export.arxiv.org/api/query?search_query={urllib.request.quote(query)}&max_results=10'
try:
    req = urllib.request.Request(url)
    resp = urllib.request.urlopen(req, timeout=15)
    data = resp.read().decode()
    root = ET.fromstring(data)
    ns = {'atom': 'http://www.w3.org/2005/Atom'}
    entries = root.findall('atom:entry', ns)
    print(f'Found {len(entries)} results')
    for i, entry in enumerate(entries[:10]):
        title = entry.find('atom:title', ns).text.strip()
        summary = entry.find('atom:summary', ns).text.strip()[:200]
        arxiv_id = entry.find('atom:id', ns).text.strip()
        print(f'{i+1}. {title}')
        print(f'   ID: {arxiv_id}')
        print()
except Exception as e:
    print(f'Error: {e}')
    import traceback
    traceback.print_exc()
\" 2>&1" in /Users/mac/Documents/Rethlas/agents/generation
 succeeded in 1594ms:
Found 0 results

exec
/bin/zsh -lc "cd /Users/mac/Documents/Rethlas/agents/generation && python3 -c \"
import urllib.request, xml.etree.ElementTree as ET

# Search arXiv API - broader query
query = 'all:relative+AND+all:projective+AND+all:resolution+AND+all:modular+AND+all:representation'
url = f'http://export.arxiv.org/api/query?search_query={urllib.request.quote(query)}&max_results=10'
try:
    req = urllib.request.Request(url)
    resp = urllib.request.urlopen(req, timeout=15)
    data = resp.read().decode()
    root = ET.fromstring(data)
    ns = {'atom': 'http://www.w3.org/2005/Atom'}
    entries = root.findall('atom:entry', ns)
    print(f'Found {len(entries)} results')
    for i, entry in enumerate(entries[:10]):
        title = entry.find('atom:title', ns).text.strip()
        summary = entry.find('atom:summary', ns).text.strip()[:300]
        arxiv_id = entry.find('atom:id', ns).text.strip()
        print(f'{i+1}. {title}')
        print(f'   ID: {arxiv_id}')
        print(f'   {summary}')
        print()
except Exception as e:
    import traceback
    traceback.print_exc()
\" 2>&1" in /Users/mac/Documents/Rethlas/agents/generation
 succeeded in 2765ms:
Found 0 results

codex
arXiv search isn't working from this environment. Let me focus on deep mathematical reasoning instead, as the instructions say. This is a well-known result that I should be able to reconstruct.

Let me think more carefully about this problem. 

The statement: "Prove that for $\overline{\mathbb{F}}_p$-representations of a finite group $G$, the trivial representation has a finite resolution by representations induced from its Sylow $p$-group."

Let me think about what specific mathematical structure this is pointing to.

**Key insight**: This is likely about the fact that the trivial module has a finite **relative projective resolution** or a **finite resolution by relatively $P$-projective modules**. But more specifically, there's a construction using the **$p$-group complex** or the **Tits building**.

Actually, wait. Let me think about the **norm map** and **cohomology** perspective.

For a Sylow $p$-subgroup $P \leq G$, there's the norm map $N_{G/P}: k \to kG \otimes_{kP} k$... no.

Let me think about this differently. There's a classical result in modular representation theory:

The trivial $kG$-module has a finite resolution by permutation modules $k[G/Q]$ where $Q$ ranges over $p$-subgroups. This is the **Webb conjecture / theorem** (proved by Symonds and others). But the theorem says the terms are $k[G/Q]$ for *some* $p$-subgroups $Q$, not necessarily all the same Sylow $p$-subgroup.

However, the problem specifically says "induced from **its** Sylow $p$-group" — singular. This means all terms are of the form $\operatorname{Ind}_P^G(V)$ where $P$ is a fixed Sylow $p$-subgroup.

Now I need to think about this more carefully. Perhaps the answer is trivial after all:

Let $P$ be a Sylow $p$-subgroup. Since $p \nmid [G:P]$, the trivial module $k$ is a direct summand of $\operatorname{Ind}_P^G(k)$. More generally, every $kG$-module is relatively $P$-projective.

Can we construct a resolution of $k$ by modules of the form $\operatorname{Ind}_P^G(X)$? Let me think about the **relative projective resolution**:

For any $kG$-module $M$ that is relatively $P$-projective (which $k$ is), we can form the relative bar resolution:
$$\cdots \to (\operatorname{Ind}_P^G \operatorname{Res}_P^G)^2(M) \to \operatorname{Ind}_P^G \operatorname{Res}_P^G(M) \to M \to 0$$

Here each term is of the form $\operatorname{Ind}_P^G(Y)$ for some $kP$-module $Y$. So this is exactly a resolution by modules induced from $P$!

The question is: is this resolution *finite*?

In general, the relative bar resolution is infinite. However, maybe there's a way to truncate it using some special property of the trivial module.

Actually wait — let me reconsider. For the trivial module $k$:

$\operatorname{Res}_P^G(k) = k$ (trivial $kP$-module)
$\operatorname{Ind}_P^G(k) = k[G/P]$

So the relative bar resolution becomes:
$$\cdots \to \operatorname{Ind}_P^G((\operatorname{Res}_P^G \operatorname{Ind}_P^G)^n(k)) \to \cdots \to \operatorname{Ind}_P^G(\operatorname{Res}_P^G \operatorname{Ind}_P^G(k)) \to \operatorname{Ind}_P^G(k) \to k \to 0$$

This is effectively:
$$\cdots \to \operatorname{Ind}_P^G((k[G/P]|_P)^{\otimes n}_k) \to \cdots$$

Hmm, this is getting complicated. Let me think about a DIFFERENT approach.

**Different approach — Mackey decomposition**: Consider the restriction of $\operatorname{Ind}_P^G(k)$ to $P$:
$$\operatorname{Res}_P^G \operatorname{Ind}_P^G(k) = \bigoplus_{g \in P \backslash G / P} \operatorname{Ind}_{P \cap gPg^{-1}}^P(k)$$

Each term is induced from a subgroup of $P$. Since $P$ is a $p$-group, each $P \cap gPg^{-1}$ is a $p$-group.

Now, there's a known result: $\operatorname{Ind}_P^G(k)$ has a finite filtration by modules $k[G/P]$... no, that's not right either.

Let me try yet another approach. The statement might be about the **relative projective resolution** of the trivial module being finite because we can use a **contractible complex** of $p$-subgroups.

Actually, let me try to think about what "finite resolution" really means here. Perhaps the resolution has length equal to the $p$-rank of $G$ or something similar.

Wait, I just thought of something. The problem might be much simpler than I'm making it. Consider the **augmentation ideal** perspective:

Let $\Delta$ be the kernel of the augmentation map $\epsilon: kG \to k$ (the augmentation ideal). Then we have a short exact sequence:
$$0 \to \Delta \to kG \to k \to 0$$

Now $kG = \operatorname{Ind}_P^G(kP)$ (since $\operatorname{Ind}_P^G(kP) = kG \otimes_{kP} kP = kG$). So $kG$ IS induced from $P$.

So we get:
$$0 \to \Delta \to \operatorname{Ind}_P^G(kP) \to k \to 0$$

Now $\Delta$ is a $kG$-module. Can we express $\Delta$ in terms of induced modules from $P$? Since every $kG$-module is relatively $P$-projective, $\Delta$ is a direct summand of $\operatorname{Ind}_P^G(\operatorname{Res}_P^G(\Delta))$. But this doesn't give us $\Delta$ itself as an induced module.

However, perhaps we can find a *surjection* from something induced from $P$ onto $\Delta$:

$\operatorname{Ind}_P^G(\operatorname{Res}_P^G(\Delta)) \twoheadrightarrow \Delta$

Let $K_1$ be the kernel. Then:
$$0 \to K_1 \to \operatorname{Ind}_P^G(\operatorname{Res}_P^G(\Delta)) \to \operatorname{Ind}_P^G(kP) \to k \to 0$$

This gives a 2-step resolution, but the first term isn't just induced from $P$ — it's induced from $P$ followed by restriction then induction. But $\operatorname{Ind}_P^G(\operatorname{Res}_P^G(\Delta))$ IS induced from $P$ (since $\operatorname{Res}_P^G(\Delta)$ is a $kP$-module).

So continuing this process: each kernel is relatively $P$-projective, so it receives a split surjection from its induced-restricted version. This gives an infinite resolution:
$$\cdots \to \operatorname{Ind}_P^G(\operatorname{Res}_P^G(K_n)) \to \cdots \to \operatorname{Ind}_P^G(kP) \to k \to 0$$

This is infinite in general. So that's not the answer.

**NEW IDEA**: Maybe "finite resolution" is achieved by using the fact that the trivial module is relatively $P$-projective AND $P$ is a Sylow $p$-subgroup, which gives a direct summand decomposition that terminates after ONE step.

The split exact sequence:
$$0 \to M \to \operatorname{Ind}_P^G(k) \to k \to 0$$

Now the question is: is $M$ induced from $P$? If $M$ itself is induced from $P$, then we'd have a resolution of length 2:
$$0 \to \operatorname{Ind}_P^G(V) \to \operatorname{Ind}_P^G(k) \to k \to 0$$

for some $V$. But $M$ is $k[G/P]/k$, which is not generally induced from $P$ (it's induced from a proper subgroup of $P$ only in special cases).

Actually, $k[G/P]$ has a filtration by modules of the form $k[G/Q]$ where $Q$ ranges over $p$-subgroups. This is the **Alperin weight conjecture** territory. But again, these aren't all induced from the same $P$.

Let me try to search for "finite resolution by modules induced from a Sylow p-subgroup" more specifically. This is a very specific claim.

**ANOTHER APPROACH**: Maybe this is a trivial consequence of the fact that every $kG$-module has a finite *injective* resolution by modules *co-induced* from $P$, and by duality (using the contragredient), we get a finite *projective* resolution by modules *induced* from $P$?

Wait, $\operatorname{Ind}_P^G$ and $\operatorname{Coind}_P^G$ are naturally isomorphic for finite groups (since $[G:P]$ is finite). So induced = coinduced.

OK, I think I need to step back and think about what theorem would give a FINITE resolution. There are only a few ways to get finite resolutions in modular representation theory:

1. **Projective resolutions**: The trivial module has a finite projective resolution iff $G$ is a $p$-group (since projective = free for $p$-groups). For non-$p$-groups, the resolution is infinite.

2. **Permutation resolutions**: The Webb-Symonds theorem says the trivial module has a finite resolution by $p$-permutation modules (permutation modules $k[G/Q]$ for $p$-subgroups $Q$).

3. **Induced from Sylow**: This is the claim we're trying to prove.

Let me think about whether Webb's theorem implies the claim. In Webb's resolution, the $i$-th term is:
$$M_i = \bigoplus_{\sigma \in \Sigma_i} k[G/G_\sigma]$$
where $\Sigma_i$ is the set of $i$-simplices in the $p$-subgroup complex, and $G_\sigma$ is the stabilizer (the smallest $p$-subgroup in the chain).

Each term $k[G/Q]$ for a $p$-subgroup $Q$ can be written as $kG \otimes_{kQ} k = \operatorname{Ind}_Q^G(k)$. But we need everything to be $\operatorname{Ind}_P^G(\text{something})$, not $\operatorname{Ind}_Q^G(\text{something})$.

But note that $\operatorname{Ind}_Q^G(k) = \operatorname{Ind}_P^G(\operatorname{Ind}_Q^P(k))$ (by transitivity of induction). So if we can write things as induced from $P$, we need the intermediate $kP$-module to be induced from $Q \leq P$.

For a $p$-subgroup $Q \leq P$, $\operatorname{Ind}_Q^P(k) = k[P/Q]$, which is a permutation module for the $p$-group $P$. And since $k[P/Q]$ is a $kP$-module, we have:
$$k[G/Q] = \operatorname{Ind}_Q^G(k) = \operatorname{Ind}_P^G(\operatorname{Ind}_Q^P(k)) = \operatorname{Ind}_P^G(k[P/Q])$$

**THIS IS THE KEY!** For ANY $p$-subgroup $Q \leq G$, we can first conjugate $Q$ into $P$ (since $P$ is a Sylow $p$-subgroup, $Q$ is contained in some conjugate of $P$). Then $k[G/Q] \cong k[G/Q^g] = \operatorname{Ind}_{Q^g}^G(k) = \operatorname{Ind}_P^G(\operatorname{Ind}_{Q^g}^P(k))$, which is induced from $P$.

More precisely: for any $p$-subgroup $Q \leq G$, there exists $g \in G$ such that $Q^g = gQg^{-1} \leq P$. Then:
$$k[G/Q] \cong k[G/Q^g] = \operatorname{Ind}_{Q^g}^G(k) = \operatorname{Ind}_P^G(\operatorname{Ind}_{Q^g}^P(k))$$

So EVERY permutation module on $p$-subgroup cosets is a module induced from the Sylow $p$-subgroup $P$!

Now, by the Webb-Symonds theorem, the trivial module $k$ has a finite resolution by permutation modules on $p$-subgroups:
$$0 \to \bigoplus_{\sigma \in \Sigma_d} k[G/G_\sigma] \to \cdots \to \bigoplus_{\sigma \in \Sigma_0} k[G/G_\sigma] \to k \to 0$$

Each $k[G/G_\sigma]$ is induced from $P$ (by the argument above), so the trivial module has a finite resolution by modules induced from $P$! QED.

But wait — I should verify that the Webb-Symonds theorem is indeed about $p$-subgroups and not something else. The theorem (Webb's conjecture) states that the trivial module has a finite resolution by $p$-permutation modules, which are direct summands of permutation modules $k[G/Q]$ for $p$-subgroups $Q$. Actually, the resolution uses the *chain complex of the poset of nontrivial $p$-subgroups*, where the terms are exactly $k[G/G_\sigma]$ for chains $\sigma$ of nontrivial $p$-subgroups.

Each $G_\sigma$ is the intersection of stabilizers of the $p$-subgroups in the chain, which is the normalizer of the largest $p$-subgroup? No, for a chain $Q_1 < Q_2 < \cdots < Q_i$, $G_\sigma = \bigcap_j N_G(Q_j)$. This might not be a $p$-subgroup itself!

Hmm, actually the terms in Webb's complex are $k[G/G_\sigma]$, which is the permutation module on the cosets of $G_\sigma$. But we need $G_\sigma$ to be a $p$-subgroup for the argument above to work. $G_\sigma = N_G(Q_1) \cap \cdots \cap N_G(Q_d)$ is not necessarily a $p$-subgroup.

However, I recall there's a different construction. Let me think about what the correct complex is.

The **Bouc complex** or the **$p$-subgroup complex**: Let $\mathcal{S}_p(G)$ be the poset of nontrivial $p$-subgroups of $G$. The reduced simplicial chain complex gives:
$$\cdots \to \bigoplus_{Q_0 < \cdots < Q_i} k[G/\bigcap_j N_G(Q_j)] \to \cdots$$

But there's also the **Brown complex** $\mathcal{T}_p(G)$ which uses chains of $p$-subgroups where the stabilizer of a chain is the intersection of the subgroups themselves, not their normalizers.

Wait, I think for the complex of nontrivial $p$-subgroups with the action by conjugation, the stabilizer of a simplex $\{Q_0, \ldots, Q_i\}$ is $N_G(Q_0) \cap \cdots \cap N_G(Q_i)$. This is not a $p$-subgroup.

However, for the **Tits building** $\Delta(G)$ for a finite group of Lie type, the simplex stabilizers ARE the parabolic subgroups, which are not $p$-subgroups either.

So maybe the resolution I need is different. Let me think about what structure gives a resolution where all terms are induced from $P$.

Actually, wait. There's a much simpler resolution. Let me reconsider.

For any $kG$-module $M$, consider the **relative projective resolution**:
$$\cdots \to \operatorname{Ind}_P^G \operatorname{Res}_P^G(J_{kP} \otimes_k \operatorname{Res}_P^G(M)) \to \operatorname{Ind}_P^G(\operatorname{Res}_P^G(M)) \to M \to 0$$

where $J_{kP}$ is the augmentation ideal of $kP$.

For $M = k$: $\operatorname{Res}_P^G(k) = k$, and $J_{kP} \otimes_k k = J_{kP}$.

So the resolution is:
$$\cdots \to \operatorname{Ind}_P^G(J_{kP}^{\otimes i}) \to \cdots \to \operatorname{Ind}_P^G(J_{kP}) \to \operatorname{Ind}_P^G(k) \to k \to 0$$

Now, the key question: is this resolution FINITE?

For a $p$-group $P$, the augmentation ideal $J_{kP}$ is nilpotent! In fact, $J_{kP}^n = 0$ for $n$ large enough (specifically, $n > \log_p(|P|)$, since the augmentation ideal of a $p$-group is the radical of $kP$ and is nilpotent).

Actually wait, $J_{kP}$ is the radical of $kP$ when $P$ is a $p$-group and $k$ has characteristic $p$. So $J_{kP}$ is nilpotent: $J_{kP}^{r} = 0$ for some $r$.

But in the relative bar resolution, the $i$-th term uses $J_{kP}^{\otimes i}$, not $J_{kP}^i$. The tensor product is over $k$, so $J_{kP}^{\otimes i}$ as a $kP$-module (with diagonal action) is not the same as $J_{kP}^i$ as an ideal. These are different.

Hmm, let me think again. Let me look up the exact definition of the relative bar resolution.

The relative bar resolution for $(G, P)$ is:
$$B_i = kG \otimes_{kP} X_i \otimes_{kP} M$$
where $X_i = (kG)^{\otimes_{kP} i}$ or something... no.

Actually the standard relative projective resolution uses:
$$P_i = kG \otimes_{kP} (J_{kP})^{\otimes_k i} \otimes_k \operatorname{Res}_P^G(M)$$

This is infinite in general.

Let me try a completely different approach. Maybe the result is that the trivial module has a **finite resolution by modules induced from $P$** in the sense of the **derived category** or the **stable module category**, and the resolution comes from the fact that $\operatorname{Ind}_P^G$ and $\operatorname{Res}_P^G$ form an adjoint pair and there's a **relative Koszul complex**.

Actually, I just realized: maybe the answer is about the **Green correspondence** and the existence of a **finite $P$-projective resolution**. Let me look at this from the perspective of the **relative cohomology**.

In the relative cohomology $H^*(G, P; k)$, which is defined using the relative projective resolution, these groups are the same as ordinary group cohomology when $P$ is a Sylow $p$-subgroup (by a theorem of Cartan-Eilenberg). But this doesn't help with finiteness.

Let me try yet another approach. The problem might have a very simple solution:

**Since $\operatorname{Ind}_P^G(k)$ is a permutation module containing the trivial module as a direct summand, we can repeatedly take the cokernel of the inclusion of the trivial module into a direct sum of copies of $\operatorname{Ind}_P^G(k)$.**

More concretely: 
$k$ is a direct summand of $\operatorname{Ind}_P^G(k)$. Let $\pi: \operatorname{Ind}_P^G(k) \to k$ be the projection. Then $\ker(\pi)$ is a summand of $\operatorname{Ind}_P^G(k)$, so $\ker(\pi)$ is relatively $P$-projective. But is $\ker(\pi)$ induced from $P$? Not necessarily.

OK wait, here's an idea. Maybe we should look at the **relative syzygy**:

For the trivial module $k$, let $\Omega_{G/P}(k)$ be the kernel of $\operatorname{Ind}_P^G(\operatorname{Res}_P^G(k)) \to k$. This is the first relative syzygy. Then we can repeat: take the relative syzygy of $\Omega_{G/P}(k)$, etc.

The question is whether $\Omega_{G/P}^n(k)$ eventually becomes induced from $P$ for some $n$, giving a finite resolution.

But for a general finite group, there's no reason to expect this.

Let me reconsider the problem statement more carefully: "the trivial representation has a finite resolution by representations induced from its Sylow $p$-group."

Perhaps "induced from its Sylow $p$-group" means induced from $P$ where $P$ is *any* Sylow $p$-subgroup. And "representations induced from its Sylow $p$-group" means direct sums of modules each of which is induced from $P$. The resolution is finite.

I'm starting to think this might follow from the **Webb theorem** on the $p$-subgroup complex, combined with the transitivity argument I gave above. Let me work this out carefully.

The Webb theorem (conjecture, proved by Symonds): The augmented chain complex of the poset of nontrivial $p$-subgroups, tensored with $k$, is a finite resolution of the trivial module $k$ by $p$-permutation modules.

More precisely: Let $\mathcal{S} = \mathcal{S}_p(G)$ be the poset of nontrivial $p$-subgroups of $G$. Consider the simplicial complex whose simplices are chains $Q_0 < Q_1 < \cdots < Q_d$ in $\mathcal{S}$. The augmented chain complex $C_*(\mathcal{S}; k) \to k \to 0$ is exact. The group $G$ acts on $\mathcal{S}$ by conjugation, making this a complex of $kG$-modules.

The $d$-th term is:
$$C_d = \bigoplus_{Q_0 < \cdots < Q_d} k[G/\operatorname{Stab}_G(Q_0, \ldots, Q_d)]$$

The stabilizer of the chain is $\bigcap_j N_G(Q_j)$.

Now, each summand $k[G/H]$ where $H = \bigcap_j N_G(Q_j)$. For this to be induced from $P$, we need $H \subseteq P$ (up to conjugation), so that $k[G/H] = \operatorname{Ind}_H^G(k) = \operatorname{Ind}_P^G(\operatorname{Ind}_H^P(k))$.

But $H$ contains the $p$-subgroups $Q_j$, so... $H$ contains $Q_0$, which is a nontrivial $p$-subgroup. So $H$ contains a $p$-subgroup but is not itself a $p$-subgroup in general. So $k[G/H]$ is NOT induced from $P$ in general.

So the Webb complex does NOT directly give what we need.

Hmm. Let me think about whether there's a different complex that works.

**The complex of cosets of $P$?** Consider the set of left cosets $G/P$. The permutation module $k[G/P]$ has a natural filtration by the distance from the identity coset in the coset graph, or by Bruhat decomposition... but $G$ is an arbitrary finite group, not necessarily a group of Lie type.

Wait, I just realized something. The problem might be about the **relative bar resolution** and the key insight is that for the trivial module specifically, the resolution becomes finite because the $P$-module structure is trivial.

Let me compute the relative bar resolution for the trivial module carefully.

The relative bar resolution for $M$ (with $M = k$):
$$B_n = kG \otimes_{kP} \underbrace{(kG \otimes_{kP} \cdots \otimes_{kP} kG)}_{n \text{ times}} \otimes_{kP} k$$

Wait no, the standard bar resolution relative to a subgroup is:
$$P_n = kG \otimes_{kP} kG \otimes_{kP} \cdots \otimes_{kP} kG \otimes_{kP} M$$
with $n+1$ copies of $kG$ (or $n$ copies of $kG \otimes_{kP}$ and then one $kG \otimes_{kP} M$).

The boundary map alternates sums of multiplications of adjacent factors.

For $M = k$:
$$P_n = kG \otimes_{kP} kG \otimes_{kP} \cdots \otimes_{kP} kG \otimes_{kP} k$$
with $n+1$ copies of $kG$.

But $\operatorname{Ind}_P^G(k) = kG \otimes_{kP} k$, so this is $\operatorname{Ind}_P^G$ of something? Let me check:
$$kG \otimes_{kP} k = \operatorname{Ind}_P^G(k)$$
$$kG \otimes_{kP} kG \otimes_{kP} k = \operatorname{Ind}_P^G(kG \otimes_{kP} k) = \operatorname{Ind}_P^G(\operatorname{Res}_P^G \operatorname{Ind}_P^G(k))$$

Indeed, $kG \otimes_{kP} kG \otimes_{kP} k = \operatorname{Ind}_P^G((kG \otimes_{kP} k)|_P)$ where $kG \otimes_{kP} k$ is viewed as a $kP$-module via the left $P$-action on $kG$.

So the relative bar resolution terms are of the form $\operatorname{Ind}_P^G(Y_n)$ where $Y_n$ is a $kP$-module. So every term IS induced from $P$. Good.

But this resolution is infinite. The question asks for a FINITE resolution.

Hmm, maybe I'm wrong and the relative bar resolution IS finite for the trivial module? Let me check.

For a $p$-group $P$, the bar resolution of the trivial $kP$-module is known to be... actually, for a $p$-group in characteristic $p$, the augmentation ideal is nilpotent, and the bar resolution can be "shortened" to the **minimal resolution** which is infinite (unless the group is trivial, in which case it's finite). 

Wait, for a finite $p$-group $P$, the group algebra $kP$ is local and has finite global dimension only if $P$ is trivial. In characteristic $p$, $kP$ has infinite global dimension (it's a non-semisimple Frobenius algebra). So the minimal projective resolution of $k$ over $kP$ is infinite. And by induction, the relative resolution over $G$ would also be infinite.

So the relative bar resolution is definitely infinite. The problem must have a different approach.

Let me try to think about what "finite" could mean in a different way. Maybe the resolution has length equal to the $p$-rank of $G$?

Actually, I just had another idea. Consider the **Heller operator** (syzygy) in the **relative stable category**. 

Actually, let me just try to look at specific examples to understand the claim.

**Example**: $G = S_3$, $p = 2$, $P = \langle (12) \rangle \cong C_2$, $k = \overline{\mathbb{F}}_2$.

The Sylow 2-subgroup is $P = \{1, (12)\}$. The trivial $kG$-module is $k$. The induced module $\operatorname{Ind}_P^G(k)$ has dimension 3 (since $[G:P] = 3$).

In characteristic 2, $S_3$ has two simple modules: the trivial module $k$ and the sign module (which is the same as the trivial module since char 2). 

The induced module $M = \operatorname{Ind}_P^G(k) \cong k[S_3/P]$. Let's analyze its structure.

As a module: $\operatorname{Ind}_P^G(k)$ has basis $1P, (13)P, (23)P$. The action:
- $(12)$ fixes $1P$ and swaps the other two (since $(12)(13) = (132)$, $(132)P = (23)P$? Let me compute more carefully.

Actually, let me just note that $k \mid \operatorname{Ind}_P^G(k)$ as a summand. The complement has dimension 2. 

Now, is $\operatorname{Ind}_P^G(k)$ itself projective? For $S_3$ in characteristic 2, $2 \nmid 3$, so the group algebra $kS_3$ is semisimple by Maschke's theorem! Wait, Maschke's theorem says $kG$ is semisimple iff $\operatorname{char} k \nmid |G|$. Here $\operatorname{char} k = 2$ and $|S_3| = 6$, so $2 \mid 6$, so $kS_3$ is NOT semisimple.

OK so the example is nontrivial. Let me compute the relative bar resolution to see if it's finite.

Actually, I realize I should try a completely different tactic. Let me search for "relative projective resolution trivial module Sylow p-subgroup finite" in the arXiv API with different queries.
exec
/bin/zsh -lc "cd /Users/mac/Documents/Rethlas/agents/generation && python3 -c \"
import urllib.request, xml.etree.ElementTree as ET

queries = [
    'all:relative+AND+all:homological+AND+all:algebra+AND+all:Sylow',
    'all:permutation+AND+all:module+AND+all:resolution+AND+all:trivial+AND+all:Sylow',
    'all:p-subgroup+AND+all:complex+AND+all:resolution',
]
for query in queries:
    url = f'http://export.arxiv.org/api/query?search_query={urllib.request.quote(query)}&max_results=5'
    try:
        req = urllib.request.Request(url)
        resp = urllib.request.urlopen(req, timeout=10)
        data = resp.read().decode()
        root = ET.fromstring(data)
        ns = {'atom': 'http://www.w3.org/2005/Atom'}
        entries = root.findall('atom:entry', ns)
        if entries:
            print(f'=== {query[:50]}... ===')
            for i, entry in enumerate(entries[:3]):
                title = entry.find('atom:title', ns).text.strip()
                arxiv_id = entry.find('atom:id', ns).text.strip()
                print(f'  {i+1}. {title}')
                print(f'     {arxiv_id}')
            print()
    except Exception as e:
        pass
print('Done')
\" 2>&1" in /Users/mac/Documents/Rethlas/agents/generation
exec
/bin/zsh -lc "cd /Users/mac/Documents/Rethlas/agents/generation && python3 -c \"
import urllib.request, xml.etree.ElementTree as ET

query = 'all:trivial+AND+all:module+AND+all:resolution+AND+all:finite+AND+all:p-group'
url = f'http://export.arxiv.org/api/query?search_query={urllib.request.quote(query)}&max_results=10'
try:
    req = urllib.request.Request(url)
    resp = urllib.request.urlopen(req, timeout=10)
    data = resp.read().decode()
    root = ET.fromstring(data)
    ns = {'atom': 'http://www.w3.org/2005/Atom'}
    entries = root.findall('atom:entry', ns)
    print(f'Found {len(entries)}')
    for i, entry in enumerate(entries[:10]):
        title = entry.find('atom:title', ns).text.strip()
        arxiv_id = entry.find('atom:id', ns).text.strip()
        print(f'{i+1}. {title}')
        print(f'   {arxiv_id}')
except Exception as e:
    print(f'Error: {e}')
\" 2>&1" in /Users/mac/Documents/Rethlas/agents/generation
 succeeded in 5444ms:
Found 0

codex
arXiv API isn't returning results. Let me try web scraping instead.
exec
/bin/zsh -lc "cd /Users/mac/Documents/Rethlas/agents/generation && python3 -c \"
import urllib.request

# Directly try to access known pages
# Try searching on math.stackexchange or similar
url = 'https://arxiv.org/search/?query=relative+projective+resolution+sylow&searchtype=all'
try:
    req = urllib.request.Request(url, headers={'User-Agent': 'Mozilla/5.0'})
    resp = urllib.request.urlopen(req, timeout=10)
    data = resp.read().decode()[:3000]
    # Extract hrefs with arXiv IDs
    import re
    ids = re.findall(r'arxiv\\.org/abs/(\\d+\\.\\d+)', data)
    print('Found arXiv IDs:', ids[:10])
except Exception as e:
    print(f'Error: {e}')
\" 2>&1" in /Users/mac/Documents/Rethlas/agents/generation
 succeeded in 30698ms:
Done

 succeeded in 10537ms:
Error: The read operation timed out

codex
Web searches are timing out. Let me pivot to deep mathematical reasoning. I have substantial knowledge of modular representation theory and should be able to construct the proof.

Let me think very carefully about what resolution works.

The key insight I keep coming back to is the **Webb conjecture/theorem** about the $p$-subgroup complex. Let me work through this more carefully.

**Revised approach using the $p$-subgroup complex:**

Let $\mathcal{S}_p(G)$ be the poset of all nontrivial $p$-subgroups of $G$, ordered by inclusion. The order complex $\Delta(\mathcal{S}_p(G))$ is a simplicial complex whose $d$-simplices are chains $Q_0 < Q_1 < \cdots < Q_d$ of nontrivial $p$-subgroups.

Let $C_d$ be the $k$-vector space with basis the set of $d$-simplices. The augmented simplicial chain complex is:
$$\cdots \to C_1 \to C_0 \to k \to 0$$

Webb's theorem/conjecture (proved by Symonds for general finite groups and by Bouc for solvable groups) states that this complex is exact (as a complex of $kG$-modules, with $G$ acting by conjugation).

Now, $C_d = \bigoplus_{Q_0 < \cdots < Q_d} k[G/\operatorname{Stab}(Q_0, \ldots, Q_d)]$ where $\operatorname{Stab}(Q_0, \ldots, Q_d) = \bigcap_{j=0}^d N_G(Q_j)$.

But as I noted, the stabilizer is NOT a $p$-subgroup in general.

HOWEVER, there's a different complex that DOES work. Consider the **set of cosets** $G/P$ and the **complex of subsets**...

Wait, actually, there's a theorem by **Bouc** that constructs a resolution using the complex of chains of $p$-subgroups where the stabilizer acts trivially... let me think differently.

Actually, I think what I need is the following simpler observation:

For any $p$-subgroup $Q$ of $G$, $Q \subseteq gPg^{-1}$ for some $g \in G$ (since $P$ is a Sylow $p$-subgroup). Then:

$k[G/Q] = \operatorname{Ind}_Q^G(k) \cong \operatorname{Ind}_{gPg^{-1}}^G(\operatorname{Ind}_Q^{gPg^{-1}}(k)) = \operatorname{Ind}_P^G(\text{some } kP\text{-module})$

The isomorphism $\operatorname{Ind}_{gPg^{-1}}^G \cong \operatorname{Ind}_P^G$ comes from the fact that conjugation by $g$ gives an isomorphism between representations induced from conjugate subgroups. Specifically:

$\operatorname{Ind}_{gPg^{-1}}^G(k) \cong \operatorname{Ind}_P^G(k)$ via the map $x \otimes_{gPg^{-1}} 1 \mapsto xg \otimes_P 1$.

More generally, for any $k[gPg^{-1}]$-module $V$, we can "conjugate" it to a $kP$-module $V^g$ (where $p \in P$ acts as $gpg^{-1}$ on $V$), and:
$\operatorname{Ind}_{gPg^{-1}}^G(V) \cong \operatorname{Ind}_P^G(V^g)$

So indeed: for any $p$-subgroup $Q$, we can write $Q \leq P^g$ for some $g$, then:
$k[G/Q] = \operatorname{Ind}_Q^G(k) = \operatorname{Ind}_{P^g}^G(\operatorname{Ind}_Q^{P^g}(k)) \cong \operatorname{Ind}_P^G((\operatorname{Ind}_Q^{P^g}(k))^{g^{-1}})$

So every $k[G/Q]$ for a $p$-subgroup $Q$ IS induced from $P$.

Now, if the trivial module has a finite resolution by direct sums of modules of the form $k[G/Q_i]$ where each $Q_i$ is a $p$-subgroup, then the trivial module has a finite resolution by modules induced from $P$. And Webb's theorem says exactly that!

BUT the issue is that Webb's theorem uses $k[G/\operatorname{Stab}(\sigma)]$, not $k[G/Q]$. The stabilizer is not necessarily a $p$-subgroup.

Wait, let me reconsider. There are in fact TWO different complexes people consider:

1. The complex where $G$ acts by conjugation on the poset of nontrivial $p$-subgroups. The stabilizer of a simplex is the intersection of normalizers, which contains the subgroups but is not a $p$-subgroup.

2. The complex of **cosets of $p$-subgroups** or the **transport category**.

Actually, let me think about what the "resolution by representations induced from its Sylow $p$-group" really means. Maybe it's about the following simple fact:

Every finite-dimensional $kG$-module has a finite resolution by relatively $P$-projective modules. And every relatively $P$-projective module is a direct summand of a module induced from $P$.

But this doesn't give a resolution by INDUCED modules (only by summands of induced modules).

OK, I think I need to take a completely different tack. Let me see if I can construct a resolution explicitly using the structure of $kG$ as a $kP$-module.

**Direct construction attempt:**

Consider $kG$ as a $(kG, kP)$-bimodule. It's free as a right $kP$-module (of rank $[G:P]$) and free as a left $kG$-module (of rank 1).

Let $\Delta_P$ be the kernel of the augmentation map $kP \to k$. Since $P$ is a $p$-group, $\Delta_P$ is nilpotent: $\Delta_P^N = 0$ for some $N$.

Consider the complex:
$$\cdots \to kG \otimes_{kP} \Delta_P^{\otimes i} \to \cdots \to kG \otimes_{kP} \Delta_P \to kG \otimes_{kP} k \to k \to 0$$

The map $kG \otimes_{kP} k \to k$ is $g \otimes 1 \mapsto 1$ (augmentation).

Now, $\Delta_P$ is the radical of $kP$ (since $kP$ is a local ring with residue field $k$). And $\Delta_P$ is nilpotent. But $\Delta_P^{\otimes i}$ (tensor product over $k$) is not the same as $\Delta_P^i$ (product inside $kP$).

However, $\Delta_P^{\otimes i}$ has a filtration where the associated graded involves $\Delta_P^j / \Delta_P^{j+1}$. Since $\Delta_P^N = 0$, perhaps $\Delta_P^{\otimes N} = 0$ in some sense? No, $\Delta_P$ as a $k$-vector space is non-zero, so $\Delta_P^{\otimes i}$ is non-zero for all $i$.

This approach doesn't give finiteness.

Let me try COMPLETELY DIFFERENT thinking. 

What if the resolution uses the following idea: Since $P$ is a Sylow $p$-subgroup, $[G:P]$ is coprime to $p$, so there exists a $P$-equivariant projection $\pi: kG \to kP$ (as right $kP$-modules). Specifically, since $kG$ is a free $kP$-module, we can pick a $kP$-basis. The trace map or averaging over coset representatives gives a projection.

Then $kG \otimes_{kP} k$ (which is $\operatorname{Ind}_P^G(k)$) can be written as... hmm.

Wait, I just thought of something. What if the resolution is simply:

$$0 \to \operatorname{Ind}_P^G(\Delta_P^{n-1}) \to \cdots \to \operatorname{Ind}_P^G(\Delta_P) \to \operatorname{Ind}_P^G(kP) \to k \to 0$$

where $\Delta_P^i$ is the $i$-th power of the augmentation ideal of $kP$, and $n$ is the nilpotency index of $\Delta_P$?

This would be a finite resolution (since $\Delta_P^n = 0$ for $n$ large enough) by modules induced from $P$! Let me check if this is exact.

Let's unwind this. We have:

$kP \supset \Delta_P \supset \Delta_P^2 \supset \cdots \supset \Delta_P^n = 0$

Each quotient $\Delta_P^i / \Delta_P^{i+1}$ is a trivial $kP$-module (since for a $p$-group, the radical layers are all trivial). So we have a filtration of $kP$ with trivial subquotients.

Now apply $\operatorname{Ind}_P^G = kG \otimes_{kP} -$. Since this functor is exact (right $kP$-modules are free, tensoring over $kP$ with a free left $kP$-module $kG$ is exact), we get a filtration of $\operatorname{Ind}_P^G(kP) = kG$:
$$kG = \operatorname{Ind}_P^G(kP) \supset \operatorname{Ind}_P^G(\Delta_P) \supset \operatorname{Ind}_P^G(\Delta_P^2) \supset \cdots \supset 0$$

The successive quotients are:
$$\operatorname{Ind}_P^G(\Delta_P^i) / \operatorname{Ind}_P^G(\Delta_P^{i+1}) \cong \operatorname{Ind}_P^G(\Delta_P^i / \Delta_P^{i+1}) \cong \operatorname{Ind}_P^G(k^{\oplus d_i}) = \operatorname{Ind}_P^G(k)^{\oplus d_i}$$

where $d_i = \dim_k \Delta_P^i / \Delta_P^{i+1}$.

So we get a long exact sequence linking these together. But this is a filtration, not a resolution. To get a resolution, we'd want to combine these short exact sequences.

For the $i$-th step of the filtration we have:
$$0 \to \operatorname{Ind}_P^G(\Delta_P^{i+1}) \to \operatorname{Ind}_P^G(\Delta_P^i) \to \operatorname{Ind}_P^G(k)^{\oplus d_i} \to 0$$

Now, $\operatorname{Ind}_P^G(k) \to k$ is split surjective (with kernel $M$). So we have $0 \to M \to \operatorname{Ind}_P^G(k) \to k \to 0$.

But $M$ is NOT of the form $\operatorname{Ind}_P^G(\Delta_P)$ or anything that simple.

Hmm, actually $\operatorname{Ind}_P^G(\Delta_P)$ is the kernel of the map $\operatorname{Ind}_P^G(kP) \to \operatorname{Ind}_P^G(k)$. The map $kP \to k$ with kernel $\Delta_P$ induces:
$$\operatorname{Ind}_P^G(\Delta_P) \to \operatorname{Ind}_P^G(kP) \to \operatorname{Ind}_P^G(k) \to 0$$
which is exact (since induction is right exact but also exact here as $kP$ is free over itself).

Wait, $\operatorname{Ind}_P^G$ is exact (it's tensoring over $kP$ with $kG$, and $kG$ is free as a right $kP$-module, so $\operatorname{Ind}_P^G$ is an exact functor). So:
$$0 \to \operatorname{Ind}_P^G(\Delta_P) \to \operatorname{Ind}_P^G(kP) = kG \to \operatorname{Ind}_P^G(k) \to 0$$
is exact.

Now, combine with $0 \to M \to \operatorname{Ind}_P^G(k) \to k \to 0$:

We have $kG \to \operatorname{Ind}_P^G(k) \to k \to 0$. 

I'm going in circles. Let me try to write down a candidate finite resolution explicitly.

**Candidate resolution:**

Define $P_i = \operatorname{Ind}_P^G(\Delta_P^i)$ for $i = 0, 1, \ldots, n$ where $\Delta_P^0 = kP$, $\Delta_P^n = 0$.

We have inclusions $\Delta_P^{i+1} \hookrightarrow \Delta_P^i$. Since $\operatorname{Ind}_P^G$ is exact:
$$0 \to P_{i+1} \to P_i \to \operatorname{Ind}_P^G(\Delta_P^i/\Delta_P^{i+1}) \to 0$$

Now, for a $p$-group $P$, $\Delta_P^i / \Delta_P^{i+1}$ is a direct sum of copies of the trivial module $k$. This is because the radical filtration of a $p$-group algebra in characteristic $p$ has all composition factors isomorphic to the trivial module (since the trivial module is the only simple $kP$-module).

So $\Delta_P^i / \Delta_P^{i+1} \cong k^{\oplus d_i}$ for some $d_i$. And $\operatorname{Ind}_P^G(k^{\oplus d_i}) = \operatorname{Ind}_P^G(k)^{\oplus d_i}$.

Now, we have:
$$0 \to P_{i+1} \to P_i \to \operatorname{Ind}_P^G(k)^{\oplus d_i} \to 0$$

And we know $\operatorname{Ind}_P^G(k) \cong k \oplus M$ for some $M$ (since $[G:P]$ is invertible in $k$).

So we have a short exact sequence:
$$0 \to P_{i+1} \to P_i \to (k \oplus M)^{\oplus d_i} \to 0$$

This doesn't directly give a resolution of $k$, but it gives a lot of exact sequences involving induced modules.

Now, here's the key idea. We can splice these exact sequences and also use the splitting of $k$ from $\operatorname{Ind}_P^G(k)$ to "peel off" the trivial summand.

Let me try a different approach entirely. Let me consider the following:

The trivial module $k$ is a direct summand of $\operatorname{Ind}_P^G(k)$. Let $e: \operatorname{Ind}_P^G(k) \to k$ be the split surjection (the augmentation divided by $[G:P]$) and $s: k \to \operatorname{Ind}_P^G(k)$ the split injection (the trace map). Then $e \circ s = \operatorname{id}_k$. 

Let $K = \ker(e)$. Then $\operatorname{Ind}_P^G(k) = k \oplus K$.

Now, $K$ is also a $kG$-module and hence relatively $P$-projective. So $\operatorname{Ind}_P^G(\operatorname{Res}_P^G(K)) \twoheadrightarrow K$ is split surjective. But this gives us $\operatorname{Ind}_P^G$ of something mapping onto $K$.

The resolution:
$$\cdots \to \operatorname{Ind}_P^G(\operatorname{Res}_P^G(K)) \to \operatorname{Ind}_P^G(k) \to k \to 0$$

is a resolution but it's infinite. The first two terms are induced from $P$, but then we need to resolve the kernel of $\operatorname{Ind}_P^G(\operatorname{Res}_P^G(K)) \to K$, which requires another induced module... this goes on forever.

**BREAKTHROUGH IDEA**: What if we don't use the relative projectivity approach but instead use the **Heller operator in the relative stable category**? Or better yet, what if the resolution is:

$$0 \to \operatorname{Ind}_P^G(k)^{\oplus d_n} \to \cdots \to \operatorname{Ind}_P^G(k)^{\oplus d_0} \to k \to 0$$

where the maps are constructed using the fact that $\operatorname{Ind}_P^G(k)$ contains $k$ as a summand, and we can build a "Koszul-type" complex?

Let me think about what such a complex would look like. We have $\operatorname{End}_{kG}(\operatorname{Ind}_P^G(k)) \cong \operatorname{Hom}_{kP}(k, \operatorname{Res}_P^G \operatorname{Ind}_P^G(k))$ by Frobenius reciprocity. And:
$$\operatorname{Res}_P^G \operatorname{Ind}_P^G(k) = \bigoplus_{g \in P\backslash G/P} \operatorname{Ind}_{P \cap gPg^{-1}}^P(k)$$

This is the Mackey decomposition. Many of the double coset terms are isomorphic to $kP$ (when $g \in N_G(P)$) or induced from proper subgroups of $P$.

I think I need to approach this differently. Let me search for "finite relative projective resolution trivial module" specifically.

Let me try a Google search via curl.
exec
/bin/zsh -lc 'curl -s -L --max-time 10 "https://www.google.com/search?q=finite+resolution+trivial+module+induced+from+Sylow+p-subgroup" -H "User-Agent: Mozilla/5.0" 2>&1 | head -100' in /Users/mac/Documents/Rethlas/agents/generation
 succeeded in 319ms:
<!DOCTYPE html><html lang="en-SG"><head><title>Google Search</title><style>body{background-color:#fff}</style><script nonce="5wz2kulV5Uv3sdz5Qfxitw">window.google = window.google || {};window.google.c = window.google.c || {cap:0};</script></head><body><noscript><style>table,div,span,p{display:none}</style><meta content="0;url=/httpservice/retry/enablejs?sei=KCgdauDQAambnesPlZDHmAg" http-equiv="refresh"><div style="display:block">Please click <a href="/httpservice/retry/enablejs?sei=KCgdauDQAambnesPlZDHmAg">here</a> if you are not redirected within a few seconds.</div></noscript><script nonce="5wz2kulV5Uv3sdz5Qfxitw">(function(){var sctm=false;(function(){sctm&&google.tick("load","pbsst");}).call(this);})();</script><script nonce="5wz2kulV5Uv3sdz5Qfxitw">//# sourceMappingURL=data:application/json;charset=utf-8;base64,eyJ2ZXJzaW9uIjogMywic291cmNlcyI6WyIiXSwic291cmNlc0NvbnRlbnQiOlsiICJdLCJuYW1lcyI6WyJjbG9zdXJlRHluYW1pY0J1dHRvbiJdLCJtYXBwaW5ncyI6IkFBQUE7QUFBQTtBQUFBO0FBQUE7QUFBQTtBQUFBO0FBQUEifQ==
(function(){var y=function(A,G,a,Z,k,V,e,n,C,d){{C=37;while(C!=20){if(C==A)return d;if(C==37)C=75;else if(C==75)C=(a&13)==a?G:14;else if(C==G)d=Z,C=14;else if(C==53){a:{if((n=(e=Z,K.trustedTypes),!n)||!n.createPolicy){d=e;break a}try{e=n.createPolicy(V,{createHTML:v,createScript:v,createScriptURL:v})}catch(N){if(K.console)K.console[k](N.message)}d=e}C=A}else C==14&&(C=(a|16)==a?53:A)}}},v=function(A){return y.call(this,22,80,5,A)},K=this||self;(0,eval)(function(A,G){return(G=y(22,80,16,null,"error","ks"))&&A.eval(G.createScript("1"))===1?function(a){return G.createScript(a)}:function(a){return""+a}}(K)(Array(Math.random()*7824|0).join("\n")+['//# sourceMappingURL=data:application/json;charset=utf-8;base64,eyJ2ZXJzaW9uIjogMywic291cmNlcyI6WyIiXSwic291cmNlc0NvbnRlbnQiOlsiICJdLCJuYW1lcyI6WyJjbG9zdXJlRHluYW1pY0J1dHRvbiJdLCJtYXBwaW5ncyI6IkFBQUE7QUFBQTtBQUFBO0FBQUE7QUFBQTtBQUFBO0FBQUEifQ==',
'(function(){/*',
'',
' Copyright Google LLC',
' SPDX-License-Identifier: Apache-2.0',
'*/',
'var Zs=function(a,C,A,G,e,K,n,Z,V,k,y,v,d){{v=27;while(v!=44)if(v==2)v=(C&109)==C?57:99;else if(v==60)n=new aa(a,this),K=A.listener,G=A.h9||A.src,A.pu&&Gf(63,5,A,"on",0),e=K.call(G,n),v=11;else if(v==61)v=(C|40)==C?82:2;else if(v==67)v=A.A9?68:60;else if(v==57)typeof a.className=="string"?a.className=G:a.setAttribute&&a.setAttribute(A,G),v=99;else if(v==27)v=74;else if(v==11)d=e,v=61;else if(v==74)v=(C<<1&7)==2?67:61;else if(v==68)e=true,v=11;else if(v==82){a:{if((e.X=((n=(V=(k=(a||e.HU++,e.J9>A&&e.o&&e.dG&&e.Lu<=1&&!e.P)&&!e.i&&(!a||e.Aa-K>1)&&document.hidden==0,e.HU==4))||k?e.J():e.ml,Z=n-e.ml,e.W+=Z>>14>A,e).F&&(e.F^=(e.W+1>>2)*(Z<<2)),e.W+1>>2!=A||e.X),V)||k)e.HU=A,e.ml=n;if(!k){d=false;break a}if(e.J9>e.dK&&(e.dK=e.J9),n-e.RE<e.J9-(G?255:a?5:2)){d=false;break a}d=(((y=Q((e.Aa=K,e),a?474:212),R)(e,212,e.B),e).A.push([dR,y,a?K+1:K,e.l,e.V]),e.i=KD,true)}v=2}else if(v==99)return d}},CD=function(a,C,A,G,e){{e=37;while(e!=a){if(e==22)return G;e==37?e=75:e==75?e=(A-8^1)<A&&(A+6^C)>=A?80:14:e==80?e=14:e==53?(this[this+""]=this,e=22):e==14&&(e=(A&15)==A?53:22)}}},Vp=function(a,C,A,G,e,K,n,Z,V){{Z=49;while(Z!=48)if(Z==42)n.classList?n.classList.remove(K):(n.classList?n.classList.contains(K):yp(69,G,K,kD(e,6,n)))&&Zs(n,36,e,Array.prototype.filter.call(kD(e,3,n),function(k){return k!=K}).join(A)),Z=36;else{if(Z==36)return V;Z==64?Z=(C|48)==C?42:36:Z==76?Z=(C<<2&6)<a&&C<<a>=-61?68:a:Z==69?(V=!!(n=e.rG,G*(n|A)+~n-(n^K)-(n|~K))&&!!(e.rK&K),Z=64):Z==49?Z=76:Z==a?Z=(C>>2&8)<2&&(C|4)>>3>=a?69:64:Z==68&&(G.classList?Array.prototype.forEach.call(A,function(k){Vp(1,49," ",0,"class",k,G)}):Zs(G,8,"class",Array.prototype.filter.call(kD("class",8,G),function(k){return!yp(68,0,k,A)}).join(" ")),Z=a)}}},S9=function(a,C,A,G,e,K,n,Z){{n=28;while(n!=35)if(n==60)n=A-a&7?90:88;else if(n==88)C.raw=G,Object.freeze&&(Object.freeze(C),Object.freeze(G)),Z=C,n=90;else if(n==90)n=(A-a^24)>=A&&(A-9|22)<A?91:43;else if(n==68)e.yn(function(V){K=V},C,G),Z=K,n=52;else if(n==91)Z=rR(null,25,0,"",C,true),n=43;else{if(n==52)return Z;n==28?n=60:n==43&&(n=(A|5)>>4?52:68)}}},qp=function(a,C,A,G,e,K,n){{n=73;while(n!=94)if(n==41)n=(G>>2&7)==1?a:98;else if(n==84)n=(G|6)>>4<1&&(G+6&6)>=3?18:41;else if(n==18)e=typeof C,K=e==A&&C!=null||e=="function",n=41;else{if(n==98)return K;n==73?n=84:n==a&&(this.type=A,this.currentTarget=this.target=C,this.defaultPrevented=this.k6=false,n=98)}}},uZ=function(a,C,A,G,e,K,n,Z,V,k,y,v,d,h){for(v=48;v!=95;)if(v==48)v=94;else if(v==49)K+=A.charCodeAt(y),K+=K<<10,K^=K>>6,v=15;else if(v==11)d=function(){},n=void 0,e=iZ(A,function(N,x){for(x=7;x!=89;)x==7?x=d?71:89:x==71&&(C&&KD(C),n=N,d(),d=void 0,x=89)},!!C),K=e[1],G=e[0],h={top:function(N,x,B,u,S,L,q){for(L=69;L!=98;)if(L==35)q(),L=98;else if(L==69)q=function(){n(function(T){KD(function(){N(T)})},B)},L=64;else{if(L==6)return u=G(B),N&&N(u),u;L==76?(S=d,d=function(){(S(),KD)(q)},L=98):L==64?L=x?27:6:L==27&&(L=n?35:76)}},pe:function(N){K&&K(N)}},v=32;else if(v==83)v=y<A.length?49:68;else if(v==94)v=(a+3^7)<a&&(a+9&31)>=a?93:53;else if(v==93)y=K=0,v=67;else if(v==67)v=83;else if(v==53)v=(a^36)>>4?6:81;else if(v==46)h=(y=xD[A.substring(0,3)+"_"])?y(A.substring(3),C,G,e,K,n,Z,V,k):c(68,C,A),v=78;else if(v==81)C.OC&&C.OC.forEach(A,void 0),v=6;else if(v==6)v=(a<<1&14)==2?11:32;else if(v==78)v=(a<<1&24)<4&&(a>>2&23)>=6?14:41;else{if(v==41)return h;if(v==68)K+=K<<3,K=(Z=K>>11,-(Z|0)+(K|Z)+(~K&Z)),k=K+(K<<15)>>>0,V=new Number((e=(n=1<<C,(n^1)-(~n^1)-3*(~n&1)+(~n|1)),(e|0)-~(k&e)+~e)),V[0]=(k>>>C)%G,h=V,v=53;else if(v==14){a:{if(typeof e==="string"){h=typeof G!=="string"||G.length!=C?-1:e.indexOf(G,A);break a}for(K=A;K<e.length;K++)if(K in e&&e[K]===G){h=K;break a}h=-1}v=41}else v==15?(y++,v=83):v==32&&(v=(a|48)==a?46:78)}},yp=function(a,C,A,G,e,K,n,Z,V,k,y,v,d,h){for(d=22;d!=86;)if(d==68)K[e++]=n,d=99;else if(d==59)d=Z<G.length?91:98;else if(d==99)Z++,d=59;else if(d==32)h=uZ(64,1,C,A,G)>=C,d=12;else if(d==87)K[e++]=n&255,n>>=A,d=68;else if(d==47)d=(a+1&7)==1?65:30;else if(d==91)n=G.charCodeAt(Z),d=94;else if(d==30)d=a+4>=1&&a-2<21?42:62;else if(d==20)K=[],Z=e=C,d=95;else if(d==39)Qp.call(this),C||LD||(LD=new lZ),this.OC=this.kN=this.qu=this.U=null,this.b3=void 0,this.XD=null,this.D9=this.Uu=false,d=47;else if(d==12)d=(a>>1&23)>=12&&(a-9&14)<5?39:47;else if(d==65){a:{for(K in G)if(e.call(void 0,G[K],K,G)){h=A;break a}h=C}d=30}else{if(d==96)return h;if(d==98)h=K,d=96;else if(d==22)d=71;else if(d==42){a:{if(!(v=A.C.j[String(e)],v)){h=true;break a}for(n=(y=(v=v.concat(),0),true);y<v.length;++y)(V=v[y])&&!V.A9&&V.capture==K&&(k=V.h9||V.src,Z=V.listener,V.pu&&Y(42,C,A.C,V),n=Z.call(k,G)!==false&&n);h=n&&!G.defaultPrevented}d=62}else d==95?d=59:d==62?d=(a<<1&11)==2?20:96:d==71?d=(a&109)==a?32:12:d==94&&(d=n>255?87:68)}},Bw=function(a,C,A,G,e,K,n,Z,V,k){for(V=46;V!=17;){if(V==40)return k;V==37?(n=typeof K,Z=n!=C?n:K?Array.isArray(K)?"array":n:"null",k=Z==e||Z==C&&typeof K.length==G,V=40):V==73?(k=!!(G.rK&e)&&!!(G.K&e)!=K&&(!(n=G.TA,-~(n|e)-(~n&e)+(~n|e))||G.dispatchEvent(Np(25,4,C,16,2,K,e)))&&!G.u3,V=55):V==15?V=(A^a)&6?55:73:V==55?V=(A&22)==A?37:40:V==46&&(V=15)}},Ww=function(a,C,A,G,e,K,n){{K=7;while(K!=83){if(K==18)return n;K==7?K=20:K==8?K=(C|6)>>4?84:46:K==29?(e=A,n=function(){return e<G.length?{done:false,value:G[e++]}:{done:true}},K=18):K==46?(n=Ra[A](Ra.prototype,{length:G,floor:G,pop:G,propertyIsEnumerable:G,console:G,parent:G,splice:G,document:G,prototype:G,stack:G,replace:G,call:G}),K=84):K==20?K=(C^a)&6?8:61:K==61?(n=cw(YD,A),K=8):K==84&&(K=((C^53)&8)<1&&((C|8)&7)>=5?29:18)}}},p=function(a,C,A,G,e,K,n){a+7>>1>=a&&a-7<<2<a&&(G=Mp(A),(G|C)+256-2*(G|128)+(G&-129)&&(G=127-~(G&127)+-128|Mp(A)<<7),n=G);while((a^53)>>3==1){if(e=Hw("call",G,"number")==="array"?G:[G],this.O)C(this.O);else try{K=!this.A.length&&!this.o,M(16,this,[Pw,[],e,C,A]),A&&!K||c(49,true,this,true,A)}catch(Z){c(11,":",this,Z),C(this.O)}if([])break}switch(!(a+7>=26&&(a>>1&16)<2)){case 0==![]:![]!=0;break;case NaN===Number(void false):for(e=(K=P(9,G),0);A>0;A--)e=e<<C|Mp(G);f(G,K,e);break}return a-6>>3==1&&(G=Ra[A.H](A.HH),G[A.H]=function(){return C},G.concat=function(Z){C=Z},n=G),n},rR=function(a,C,A,G,e,K,n,Z,V,k,y,v){{v=60;while(v!=48)if(v==67){a:{if(this.n===0){y=[0,0];break a}y=(this.v.sort(function(d,h){return d-h}),[this.n,this.v[this.v.length>>1]])}v=42}else if(v==3)G=Math.floor(Math.random()*this.n),G<50&&(this.v[G]=A),v=82;else if(v==36)v=this.v.length<50?92:3;else if(v==20){a:if(Z=window.btoa){for(n=(k=A,G);k<e.length;k+=8192)n+=String.fromCharCode.apply(a,e.slice(k,k+8192));y=(V=Z(n),K&&(V=V.replace(/\\+/g,"-").replace(/\\//g,"_").replace(/=/g,G)),V);break a}v=55}else if(v==92)this.v.push(A),v=82;else if(v==51)v=(C-7|17)>=C&&(C+2^32)<C?84:82;else if(v==55)v=(C&37)==C?40:51;else if(v==84)this.n++,v=36;else{if(v==82)return y;v==26?v=(C|72)==C?67:42:v==60?v=26:v==42?v=C-1>>3==3?20:55:v==40&&(y=G in j9?j9[G]:j9[G]=A+G,v=51)}}},gR=function(a,C,A,G,e,K,n,Z,V){{Z=97;while(Z!=a)if(Z==98)Z=A<<1&10?68:29;else if(Z==29){a:{if(e=(G=J.trustedTypes,null),!G||!G.createPolicy){V=e;break a}try{e=G.createPolicy("ks",{createHTML:U4,createScript:U4,createScriptURL:U4})}catch(k){if(J.console)J.console[C](k.message)}V=e}Z=68}else{if(Z==57)return V;Z==97?Z=98:Z==9?Z=(A+8&24)>=A&&(A-9^11)<A?56:57:Z==56?(this.listener=K,this.proxy=null,this.src=G,this.type=e,this.capture=!!n,this.h9=C,this.key=++mO,this.A9=this.pu=false,Z=57):Z==43?(this.pQ=C,Z=9):Z==68&&(Z=(A^9)&10?9:43)}}},kD=function(a,C,A,G,e,K,n,Z,V,k){{k=9;while(k!=3)if(k==58){a:{for(Z=a;Z<K.length;++Z)if(n=K[Z],!n.A9&&n.listener==e&&n.capture==!!A&&n.h9==G){V=Z;break a}V=-1}k=12}else if(k==0)k=(C^38)<38&&(C|7)>=29?58:12;else{if(k==59)throw Error("Invalid decorator function "+a);if(k==9)k=14;else if(k==32)k=A?85:67;else if(k==85)k=typeof a!=="function"?59:83;else{if(k==67)throw Error("Invalid class name "+A);if(k==75)V=A.classList?A.classList:bZ(5,A,a,"string",17).match(/\\S+/g)||[],k=0;else if(k==14)k=C-9>>3==1?32:83;else if(k==83)k=(C^9)<20&&(C^19)>=3?75:0;else if(k==12)return V}}}},pD=function(a,C,A,G,e){for(e=3;e!=98;)if(e==23)e=((C|7)&15)>=12&&(C<<1&8)<1?82:49;else if(e==49)e=(C+5&67)<C&&(C+2&29)>=C?97:15;else{if(e==87)return G;if(e==97){a:{if(typeof A==="function"){G=A;break a}G=A[A[fD]||(A[fD]=function(K){return A.handleEvent(K)}),fD]}e=15}else e==82?(this.u3=this.u3,this.FD=this.FD,e=49):e==15?e=(C-a^15)<C&&(C-5|17)>=C?1:87:e==1?(this[this+""]=this,G=Promise.resolve(),e=87):e==3&&(e=23)}},FP=function(a,C,A,G,e,K,n,Z,V,k,y,v,d,h,N){{N=89;while(N!=42)if(N==39)N=(K+3^22)>=K&&(K+5^9)<K?55:7;else if(N==7)N=(K|6)>=-65&&(K<<1&8)<4?62:77;else if(N==92)Ds++,N=77;else{if(N==40)throw Error("addEventListener and attachEvent are unavailable.");if(N==71)N=A===""||A==void 0?26:47;else if(N==28)V.addListener(v),N=92;else if(N==83)Z=$D,C in Z?G.setAttribute(V,Z[C]):G.removeAttribute(V),N=7;else if(N==2)wR||(n=d),n===void 0&&(n=a),V.addEventListener(Z.toString(),v,n),N=92;else if(N==98)N=V.addEventListener?2:11;else if(N==11)N=V.attachEvent?88:57;else if(N==62)N=Z?70:59;else if(N==88)V.attachEvent(rR(null,33,"on",Z.toString()),v),N=92;else if(N==21)N=k.proxy?77:61;else if(N==55)Array.isArray(A)&&(A=A.join(" ")),V="aria-"+C,N=71;else if(N==80)n={},$D=(n.atomic=false,n.autocomplete="none",n.dropeffect="none",n.haspopup=false,n.live=e,n.multiline=false,n.multiselectable=false,n.orientation="vertical",n.readonly=false,n.relevant="additions text",n.required=false,n.sort="none",n.busy=false,n.disabled=false,n.hidden=false,n.invalid=a,n),N=83;else if(N==61)v=O4(88,8),k.proxy=v,v.src=V,v.listener=k,N=98;else{if(N==77)return h;if(N==89)N=39;else if(N==26)N=$D?83:80;else if(N==70)d=qp(69,n,e,9)?!!n.capture:!!n,(y=M(86,V))||(V[XP]=y=new zf(V)),k=y.add(Z,A,C,d,G),N=21;else{if(N==59)throw Error("Invalid event type");N==47?(G.setAttribute(V,A),N=7):N==57&&(N=V.addListener&&V.removeListener?28:40)}}}}},to=function(a,C,A,G,e,K,n,Z,V,k,y,v){for(y=68;y!=49;)if(y==a)to(83,17,"object",true,e,K,n[k],Z,V),y=45;else if(y==5)y=(C&124)==C?71:16;else if(y==43)K=pD(6,3,K),Z&&Z[s4]?Z.C.add(String(n),K,G,qp(69,V,A,8)?!!V.capture:!!V,e):FP(false,G,K,e,"object",16,V,n,Z),y=61;else if(y==92)this.n++,G=A-this.Y,this.Y+=G/this.n,this.e7+=G*(A-this.Y),y=97;else if(y==97)y=(C^56)>=-40&&(C>>1&6)<2?27:61;else if(y==17)y=k<n.length?a:61;else{if(y==61)return v;y==82?(k=0,y=95):y==27?y=Array.isArray(n)?82:43:y==68?y=5:y==16?y=(C|5)>>4?97:92:y==45?(k++,y=17):y==95?y=17:y==71&&(v=A,y=16)}},O=function(a,C,A,G,e,K,n){{e=79;while(e!=61)if(e==17)e=(a|24)==a?27:12;else if(e==54)A.A9=true,A.listener=C,A.proxy=C,A.src=C,A.h9=C,e=46;else if(e==10)e=a-6<19&&((a|4)&15)>=10?42:17;else if(e==1)K=S9(2,C,18,C),e=48;else{if(e==46)return K;e==47?(K=C&&C.parentNode?C.parentNode.removeChild(C):null,e=10):e==12?e=(a+9&16)<10&&(a-4&9)>=7?1:48:e==48?e=(a|64)==a?54:46:e==63?e=(a>>2&7)==4?47:10:e==27?(n=function(){},n.prototype=G.prototype,A.R=G.prototype,A.prototype=new n,A.prototype.constructor=A,A.m7=function(Z,V,k){for(var y=74;y!=4;)if(y==74)var v=Array(arguments.length-(y=60,C)),d=C;else if(y==88)y=d<arguments.length?33:30;else if(y==33)v[d-C]=arguments[d],y=9;else if(y==9)d++,y=88;else{if(y==30)return G.prototype[V].apply(Z,v);y==60&&(y=88)}},e=12):e==42?e=17:e==79&&(e=63)}}},bZ=function(a,C,A,G,e,K,n,Z,V,k,y){{y=79;while(y!=43)if(y==79)y=47;else if(y==47)y=e+1>>2<e&&(e-7^13)>=e?25:95;else if(y==17){for(Z in(V=A,C).j){for(K=C.j[n=A,Z];n<K.length;n++)++V,O(70,G,K[n]);delete (C.fu--,C.j)[Z]}y=21}else if(y==9)k=typeof C.className==G?C.className:C.getAttribute&&C.getAttribute(A)||"",y=73;else if(y==73)y=e-4&a?21:17;else{if(y==21)return k;y==95?y=e>>1&7?73:9:y==25&&(K=A,K^=K<<13,K=(n=K>>17,(K|n)+~(K&n)- -1),K=(Z=K<<a,(K|0)-(K&Z)-(~K^Z)+(~K|Z)),(K&=C)||(K=1),k=G^K,y=95)}}},X=function(a,C,A,G,e,K,n,Z,V,k,y,v){if((a<<1&14)>=((a^39)>>3==2&&(y=v=function(){{var d=80;while(d!=86)if(d==51)var h=(d=13,[K8,n,e,void 0,Z,V,arguments]);else if(d==89){var N=c(56,(M(20,G,h),true),G,false,false);d=29}else{if(d==29)return N;if(d==13)d=K==C?89:22;else if(d==61)d=G.u?51:27;else if(d==80)d=G.X==G?61:86;else if(d==60)N=ac(h,"load",6,G),d=29;else if(d==22)d=K==A?67:60;else if(d==67){var x=!G.A.length&&!G.o;d=(M(96,G,h),x&&c(48,true,G,false,false),29)}else d==27&&(Z&&V&&Z.removeEventListener(V,v,vI),d=86)}}}),9)&&a-1<27)a:{if(C.P){y=E4(C.G,C);break a}y=((G=C8(true,8,C),(G|0)+~G-~(G|128)-(G^128))&&(G^=128,A=C8(true,2,C),G=(G<<2)+(A|0)),G)}return(((a>>2&7)==1&&(V=K&7,n=yf,G=[-93,-47,-90,-14,-6,7,G,-54,40,54],k=Ra[e.H](e.mt),k[e.H]=function(d){V+=6+7*(Z=d,K),V&=7},k.concat=function(d,h,N,x){return(Z=(x=A%16+C,d=+(n()|0)*x+G[V+27&7]*A*x-x*Z-164*A*A*Z- -2993*Z+41*Z*Z+4*A*A*x- -1927*A*Z+V,h=G[d],void 0),G)[(N=V+21,(N|0)+7-(N|7))+(-2-2*~(K|2)+2*(~K^2)+(~K&2))]=h,G[V+(-~(K&2)+(K&-3)+(~K|2))]=-47,h},y=k),a)|4)>>3==3&&(y=(e=(V=(Z=G[A]<<24,n=G[-3*~A+-2+2*(~A^1)+C*(~A&1)]<<16,(n|0)+(Z^n)-(~Z&n)),k=G[(A|0)+2]<<8,-2-~V-(V|~k)),K=G[(A|0)+3],~e-2*~K+3*(e&~K)+(~e^K))),y},Y=function(a,C,A,G,e,K,n,Z){for(n=32;n!=88;)if(n==32)n=34;else if(n==45)b(A,G,C),C[n8]=2796,n=14;else if(n==14)n=(a+5&27)<a&&(a+5^6)>=a?7:51;else if(n==39)dr.call(this),this.C=new zf(this),this.I$=this,this.IE=null,n=60;else if(n==97)n=A.j[e].length==0?86:42;else if(n==34)n=((a|9)&5)>=3&&(a+9&12)<7?45:14;else if(n==28)n=e in A.j&&Y(27,C,G,A.j[e])?63:42;else if(n==51)n=(a|6)>>3>=2&&(a+1&8)<8?39:60;else if(n==86)delete A.j[e],A.fu--,n=42;else{if(n==42)return Z;n==7?(K=uZ(66,C,0,A,G),(e=K>=0)&&Array.prototype.splice.call(G,K,C),Z=e,n=51):n==60?n=((a^59)&15)==1?82:42:n==82?(e=G.type,n=28):n==63&&(O(71,null,G),n=97)}},O4=function(a,C,A,G,e,K,n,Z,V,k,y,v,d){{v=42;while(v!=57)if(v==18)v=K&&K.once?35:77;else if(v==77)v=Array.isArray(e)?63:82;else if(v==65)v=k<e.length?a:90;else if(v==31)d=function(h){return A.call(d.src,d.listener,h)},A=oc,y=d,v=8;else if(v==95)k++,v=65;else if(v==8)v=(C-7^10)<C&&(C+7&41)>=C?18:90;else if(v==63)k=G,v=60;else if(v==82)Z=pD(6,6,Z),n&&n[s4]?n.C.add(String(e),Z,false,qp(69,K,A,8)?!!K.capture:!!K,V):FP(false,false,Z,V,"object",11,K,e,n),v=90;else if(v==35)to(83,18,A,true,V,Z,e,n,K),v=90;else if(v==60)v=65;else if(v==40)y=Object.prototype.hasOwnProperty.call(A,Vf)&&A[Vf]||(A[Vf]=++Nh),v=38;else if(v==42)v=96;else if(v==a)O4(88,5,"object",0,e[k],K,n,Z,V),v=95;else if(v==96)v=(C&62)==C?31:8;else{if(v==38)return y;v==90&&(v=(C|48)==C?40:38)}}},P=function(a,C,A,G,e){if((a-9^18)<a&&(a-2^6)>=a)a:{for(0;C.P;0){e=E4(C.G,C);break a;if({})break}e=(((A=C8(true,8,C),A)|0)+~A-~(A|128)-(A^128)&&(A^=128,G=C8(true,2,C),A=(A<<2)+(G|0)),A)}for(undefined;a>>1<12&&(a>>2&3)>=0;1){a:{while(C.P){e=E4(C.G,C);break a;if(-0===0)break}e=((G=C8(true,8,C),(G|0)+~G-~(G|128))-(G^128)&&(G^=128,A=C8(true,2,C),G=(G<<2)+(A|0)),G)}if(true)break}return e},Np=function(a,C,A,G,e,K,n,Z,V,k,y,v,d,h){for(d=82;d!=19;)if(d==92)d=v?31:35;else if(d==96)d=a-7>>4?91:85;else{if(d==91)return h;if(d==45)v=M(82,e),d=92;else if(d==48)k++,d=1;else if(d==53)k=A,d=29;else if(d==57){a:{switch(n){case 1:h=K?"disable":"enable";break a;case e:h=K?"highlight":"unhighlight";break a;case C:h=K?"activate":"deactivate";break a;case A:h=K?"select":"unselect";break a;case G:h=K?"check":"uncheck";break a;case 32:h=K?"focus":"blur";break a;case 64:h=K?"open":"close";break a}throw Error("Invalid component state");}d=96}else d==85?(this.X=C,d=91):d==75?(y=qp(69,n,C,14)?!!n.capture:!!n,Z=pD(6,7,Z),d=34):d==24?d=Array.isArray(K)?53:75:d==82?d=99:d==30?d=(a&35)==a?24:35:d==31?((V=v.BU(K,y,G,Z))&&Gf(63,6,V,"on",0),d=35):d==1?d=k<K.length?40:35:d==87?d=e?45:35:d==17?(e.C.remove(String(K),Z,y,G),d=35):d==29?d=1:d==39?(this.Cu=J.document||document,d=30):d==99?d=(a|40)==a?39:30:d==34?d=e&&e[s4]?17:87:d==35?d=(a-6|10)>=a&&(a+3&57)<a?57:96:d==40&&(Np(3,"object",0,G,e,K[k],n,Z),d=48)}},eC=function(a,C,A,G,e,K,n,Z,V,k,y,v,d,h){for(v=(y=69,66);;)try{if(y==13)break;else if(y==27)v=66,c(7,C,n,h),y=79;else if(y==4)y=(A|32)==A?55:93;else if(y==79)v=66,y=33;else if(y==68)d=k,y=4;else if(y==33)y=K&&n.i?88:38;else if(y==11)v=94,k=ac(V,"load",e,n),y=79;else if(y==62)y=14;else if(y==69)y=28;else if(y==55)G=C,Ag===void 0&&(Ag=a),e=(K=Ag)?K.createScriptURL(G):G,d=new G7(e),y=93;else if(y==38)y=14;else if(y==88)Z=n.i,Z(function(){c(60,a,n,a,a)}),y=68;else if(y==14)y=n.A.length?90:68;else if(y==28)y=A+5>>1<A&&(A-5|31)>=A?62:4;else{if(y==93)return d;y==90&&(n.i=G,V=n.A.pop(),y=11)}}catch(N){if(v==66)throw N;v==94&&(h=N,y=27)}},Gf=function(a,C,A,G,e,K,n,Z,V,k,y){{y=19;while(y!=50)if(y==46)y=n<G.length?24:13;else if(y==69)K.src=null,n[XP]=null,y=1;else if(y==91)n=A.src,y=66;else if(y==66)y=n&&n[s4]?77:7;else if(y==25)y=typeof A!=="number"&&A&&!A.A9?91:1;else if(y==24)Z+=e,A=A<<e|G[n],y=93;else if(y==97)y=K.fu==e?69:1;else if(y==19)y=98;else if(y==58)y=K?99:26;else if(y==26)O(68,null,A),y=1;else if(y==13)k=K,y=17;else if(y==99)Y(26,1,K,A),y=97;else if(y==7)V=A.proxy,Z=A.type,n.removeEventListener?n.removeEventListener(Z,V,A.capture):n.detachEvent?n.detachEvent(rR(null,5,G,Z),V):n.addListener&&n.removeListener&&n.removeListener(V),Ds--,K=M(81,n),y=58;else{if(y==17)return k;y==a?(n=0,K=[],Z=0,y=31):y==37?(n++,y=46):y==1?y=((C|2)&5)==1?a:17:y==22?y=Z>7?80:37:y==77?(Y(10,1,n.C,A),y=1):y==98?y=C-3>>3?1:25:y==27?y=22:y==31?y=46:y==93?y=22:y==80&&(Z-=8,K.push(A>>Z&255),y=27)}}},Ia=function(a,C,A,G,e,K,n,Z,V,k,y){for(k=77;k!=99;)if(k==76)k=C-2<<2>=C&&(C-6|76)<C?58:35;else if(k==36)k=C+5>>3==3?94:86;else if(k==77)k=5;else if(k==58)k=35;else if(k==40){for(e in(K={},Array.prototype.forEach.call(kD(a,5,A),function(v){K[v]=true}),Array.prototype).forEach.call(G,function(v){K[v]=true}),n="",K)n+=n.length>0?" "+e:e;k=(Zs(A,32,a,n),36)}else if(k==19)k=63;else if(k==75){a:{if(e=G.length,e>A){for(n=(K=Array(e),A);n<e;n++)K[n]=G[n];y=K;break a}y=[]}k=46}else if(k==63)k=Z<V?17:86;else if(k==39)y=Math.floor(this.Z9+(this.J()-this.RE)),k=76;else if(k==1)Z++,k=63;else if(k==94)V=e.length,n=typeof e==="string"?e.split(A):e,Z=G,k=19;else if(k==5)k=(C&88)==C?75:46;else if(k==46)k=(C|80)==C?34:36;else if(k==86)k=C-7>>3>=2&&(C-9&14)<2?39:76;else if(k==17)Z in n&&K.call(void 0,n[Z],Z,e),k=1;else if(k==34)k=A.classList?43:40;else if(k==43)Array.prototype.forEach.call(G,function(v,d,h){for(h=82;h!=24;)h==71?(d=bZ(5,A,a,"string",16),Zs(A,12,a,d+(d.length>0?" "+v:v)),h=24):h==89?(A.classList.add(v),h=24):h==82?h=A.classList?89:28:h==28&&(h=(A.classList?A.classList.contains(v):yp(36,0,v,kD(a,7,A)))?24:71)}),k=36;else if(k==35)return y},M=function(a,C,A,G,e){for(e=75;e!=32;)if(e==5)e=((a^7)&11)==3?71:50;else if(e==92)e=(a&46)==a?46:27;else if(e==71)C.A.splice(0,0,A),e=50;else if(e==75)e=53;else if(e==50)e=(a|80)==a?58:6;else if(e==27)e=(a+1&13)==4?89:5;else if(e==46)G=rR(null,26,0,"",C,false),e=27;else if(e==60)kt.call(this,C?C.type:""),this.relatedTarget=this.currentTarget=this.target=null,this.button=this.screenY=this.screenX=this.clientY=this.clientX=this.offsetY=this.offsetX=0,this.key="",this.charCode=this.keyCode=0,this.metaKey=this.shiftKey=this.altKey=this.ctrlKey=false,this.state=null,this.pointerId=0,this.pointerType="",this.timeStamp=0,this.Qn=null,C&&this.init(C,A),e=92;else if(e==89)G=Math.floor(this.J()),e=5;else if(e==53)e=a>>2<6&&(a<<2&7)>=4?60:92;else{if(e==6)return G;e==58&&(A=C[XP],G=A instanceof zf?A:null,e=6)}},uF=function(a,C,A,G,e,K,n,Z,V,k,y,v){{v=86;while(v!=32)if(v==87)k=O4(88,51,Z),v=79;else if(v==90)v=(A+2&59)<A&&(A+8^26)>=A?81:73;else if(v==73)v=(A|2)>>4?69:25;else if(v==8)v=(n=G)?29:75;else if(v==50)v=78;else if(v==79)v=(V=hg[k])?56:94;else if(v==98)n=V?typeof V.Ku==="function"?V.Ku():new V:null,v=29;else if(v==75)Z=this.constructor,v=a;else if(v==56)v=98;else{if(v==69)return y;v==81?(BI.call(this,e),v=8):v==25?(C.Ku=function(){return C.UC?C.UC:C.UC=new C},C.UC=void 0,v=69):v==86?v=4:v==a?v=78:v==94?(Z=(K=Object.getPrototypeOf(Z.prototype))&&K.constructor,v=50):v==51?(iF.call(this,C,G||xt.Ku(),e),v=90):v==4?v=(A^21)&13?90:51:v==29?(this.T=n,v=73):v==78&&(v=Z?87:98)}}},c=function(a,C,A,G,e,K,n,Z,V,k,y){for(k=51;k!=10;)if(k==51)k=46;else if(k==46)k=(a&61)==a?25:13;else if(k==25){a:if(A.A.length){(A.o&&":TQR:TQR:"(),A.o=C,A).dG=e;try{V=A.J(),A.dK=0,A.ml=V,A.RE=V,A.HU=0,Z=eC(true,":",6,null,6,e,A),n=G?0:10,K=A.J()-A.RE,A.Z9+=K,A.sC&&A.sC(K-A.D,A.l,A.V,A.dK),A.l=false,A.V=false,A.D=0,K<n||A.Mu--<=0||(K=Math.floor(K),A.l3.push(K<=254?K:254))}finally{A.o=false}y=Z;break a}k=13}else if(k==78)k=(a|8)>>3==1?56:1;else if(k==13)k=(a&77)==a?47:78;else if(k==1)k=a+2>>4<3&&(a|5)>=21?29:84;else if(k==29)k=84;else if(k==65)this.src=C,this.j={},this.fu=0,k=9;else if(k==84)k=(a-4^28)<a&&(a+3^13)>=a?65:9;else if(k==47)C(function(v){v(A)}),y=[function(){return A},function(){}],k=78;else{if(k==9)return y;k==56&&(A.O=((A.O?A.O+"~":"E:")+G.message+C+G.stack).slice(0,2048),k=1)}},SC=function(a,C,A,G,e,K,n,Z,V,k,y,v,d,h){{v=87;while(v!=13)if(v==44)e++,v=2;else if(v==70)C.removeAttribute("srcdoc"),v=78;else if(v==73)k++,v=27;else if(v==39)v=a>>1<16&&((a^4)&14)>=0?45:51;else if(v==45)d=function(N){N&&Z.appendChild(typeof N==="string"?n.createTextNode(N):N)},k=1,v=63;else if(v==87)v=39;else if(v==66){if(A instanceof G7)K=A.pQ;else throw Error("");v=(C.src=K.toString(),19)}else if(v==98)v=(a&111)==a?70:19;else if(v==27)v=k<K.length?23:51;else if(v==43)C.sandbox.supports&&!C.sandbox.supports(G[e])||C.sandbox.add(G[e]),v=44;else if(v==2)v=e<G.length?43:66;else if(v==94)v=2;else if(v==34)G="allow-same-origin allow-scripts allow-forms allow-popups allow-popups-to-escape-sandbox allow-storage-access-by-user-activation".split(" "),C.setAttribute("sandbox",""),e=0,v=94;else if(v==46)v=!Bw(29,"object",6,"number",G,V)||qp(69,V,"object",15)&&V.nodeType>e?89:76;else{if(v==19)return h;if(v==76){a:{if(V&&typeof V.length=="number"){if(qp(69,V,"object",9)){y=typeof V.item=="function"||typeof V.item==C;break a}if(typeof V==="function"){y=typeof V.item=="function";break a}}y=false}v=(Ia("class",19,A,e,y?Ia("class",8,e,V):V,d),73)}else if(v==67)h=this.n===0?0:Math.sqrt(this.e7/this.n),v=98;else if(v==23)V=K[k],v=46;else if(v==89)d(V),v=73;else{if(v==32)throw new rr(typeof A,1);v==51?v=(a<<1&8)<8&&((a|9)&7)>=0?67:98:v==78?v=A instanceof G7?34:32:v==63&&(v=27)}}}},b=function(a,C,A){if(C==212||C==474)a.u[C]?a.u[C].concat(A):a.u[C]=p(18,A,a);else{if(a.x6&&C!=399)return;C==263||C==177||C==431||C==140||C==260||C==440||C==143||C==374||C==131||C==348?a.u[C]||(a.u[C]=X(36,1,C,A,a,62)):a.u[C]=X(37,1,C,A,a,9)}C==399&&(a.F=C8(false,32,a),a.L=void 0)},Yt=function(a,C,A,G,e,K,n,Z,V){for(true;a.X==a;Number()===-Number()){{e=Q(a,C),C==177||C==131||C==140?(n=function(k,y,v,d,h,N,x,B){for(x=(B=72,97);;)try{if(B==28)break;else if(B==19)e.push(e.gK[v&7]^k),B=28;else if(B==72)v=e.length,h=(v|0)-4>>3,B=86;else{if(B==26)throw x=97,N;B==48?(x=29,e.gK=cI(3,X(25,4,2*(d&4)-~d+-5+2*(~d&4),e),X(27,4,d,e),29,y),B=19):B==86?B=e.wK!=h?11:19:B==11&&(d=(h<<3)-4,y=[0,0,Z[1],Z[2]],e.wK=h,B=48)}}catch(u){if(x==97)throw u;x==29&&(N=u,B=26)}},Z=Q(a,374)):n=function(k){e.push(k)},G&&n(255-~(G&255)+-256),V=A.length,K=0;while(K<V)n(A[K]),K++}if(true!=![])break}},U4=function(a){return to.call(this,83,20,a)},Rc=function(a,C,A,G,e,K){(K=U(G,212),G).j7&&K<G.B?(f(G,212,G.B),L8(0,143,212,e,G,C)):b(G,212,e);while(NaN!==NaN)if(lF(14,1,A,G),{})break;return I((f(G,212,K),a),G)},Qf=function(){return Ia.call(this,"class",3)},Mh=function(a,C,A,G){(G=(A=P(22,C),X)(14,C),Yt)(C,G,WI(I(A,C),a))},oc=function(a,C,A,G,e,K){return Zs.call(this,C,17,a,A,G,e,K)},qh=function(a,C,A,G,e,K,n,Z,V,k,y,v){if((C.push((e=(G=a[0]<<24,A=a[1]<<16,(G|0)-(~G^A)+(~G|A))|a[2]<<8,V=a[3],-~V+(e^V)+(e|~V))),C).push((n=(k=a[4]<<24,v=a[5]<<16,~(k&v)-2*~(k|v)-(k&~v)+(k|~v))|a[6]<<8,Z=a[7],-1-~Z-(~n^Z)+(n|~Z))),"f")C.push((K=a[8]<<24|a[9]<<16,y=a[10]<<8,(y|0)- -1+2*(K&~y)+(~K|y))|a[11])},U=function(a,C,A){A=a.u[C];for(0;A===void 0;0==![""]){throw[T7,30,C];if(true)break}if(A.value)return A.create();return A.create(C*4*C+-47*C+-73),A.prototype},HI=function(a){return Ww.call(this,27,18,a)},R=function(a,C,A){if(C==212||C==474)a.u[C]?a.u[C].concat(A):a.u[C]=p(14,A,a);else{if(a.x6&&C!=399)return;C==263||C==177||C==431||C==140||C==260||C==440||C==143||C==374||C==131||C==348?a.u[C]||(a.u[C]=X(69,1,C,A,a,62)):a.u[C]=X(38,1,C,A,a,9)}C==399&&(a.F=C8(false,32,a),a.L=void 0)},PI,kt=function(a,C){return qp.call(this,69,C,a,5)},gr=function(a,C,A,G,e,K,n,Z,V){V=this;try{mx(G,this,n,C,a,Z,e,K,A)}catch(k){c(6,":",this,k),G(function(y){y(V.O)})}},p8=function(a,C,A,G,e,K){return uZ.call(this,9,C,a,A,G,e,K)},U5=function(a,C){for(var A=64;A!=51;)if(A==68)A=27;else if(A==25)n++,A=24;else if(A==24)A=n<arguments.length?20:51;else if(A==88){var G=Fq[K];A=(Object.prototype.hasOwnProperty.call(e,G)&&(a[G]=e[G]),90)}else if(A==20){var e=arguments[n];for(G in e)a[G]=e[G];var K=(A=68,0)}else if(A==31)A=24;else if(A==64)var n=(A=31,1);else A==27?A=K<Fq.length?88:25:A==90&&(K++,A=27)},z7=function(a,C){function A(){this.v=(this.n=0,[])}return C=((A.prototype.t9=function(G,e){return rR.call(this,null,39,G,e)},A).prototype.X7=function(){return rR.call(this,null,72)},a=new A,new A),[function(G){(a.t9(G),C).t9(G)},function(G){return C=(G=a.X7().concat(C.X7()),new A),G}]},wr=function(a,C,A,G,e,K,n){$t((((A=Q(C,(K=X(15,(G=(a|0)+3- -1+~(a|(e=a&4,3)),C)),n=P(34,C),K)),e)&&(A=Jg(""+A,224)),G)&&Yt(C,n,WI(A.length,2)),A),4,C,n)},f8=function(){for(var a=87;a!=6;)if(a==87)var C=Number((a=49,this)),A=C,G=[];else if(a==2)G[A-C]=arguments[A],a=95;else if(a==95)A++,a=8;else{if(a==25)return G;a==49?a=8:a==8&&(a=A<arguments.length?2:25)}},DE=function(){return c.call(this,18)},Jg=function(a,C,A,G,e,K,n,Z,V,k,y,v,d){K=(A=a.replace(/\\r\\n/g,"\\n"),[]);{d=V=0;while(d<A.length)n=A.charCodeAt(d),n<128?K[V++]=n:(n<2048?K[V++]=(Z=n>>6,192+~(Z&192)-~Z):((n&64512)==55296&&d+1<A.length&&(e=A.charCodeAt(d+1),(e|64512)- -1-(e&-64513)+(e|-64513))==56320?(n=(y=1023-~n+~(n|1023)<<10,2*(65536&y)+(65536&~y)-(-65537^y)+(-65537|y))+(G=A.charCodeAt(++d),1023+(G&-1024)-(G^1023)),K[V++]=n>>18|240,K[V++]=n>>12&63|128):K[V++]=(v=n>>12,(v&C)+~(v&C)-~(v|C)),K[V++]=(k=n>>6&63,(k&128)-1+(k&-129)-(k|-129))),K[V++]=63+(~n^63)-(~n|63)|128),d++}return K},rr=function(a,C,A){this.type=("stack"in(this.message=(A=Error.call(this,a+" cannot be used with intent "+Xq[C]),A.message),A)&&(this.stack=A.stack),a),this.name="TypeCannotBeUsedWithIframeIntentError"},lZ=function(){return Np.call(this,40)},E4=function(a,C,A){return(A=a.create().shift(),C.P.create().length||C.G.create().length)||(C.P=void 0,C.G=void 0),A},zf=function(a){return c.call(this,55,a)},cI=function(a,C,A,G,e,K,n,Z){K=e[2]|(n=e[a]|0,0);{Z=0;while(Z<16)C=C>>>8|C<<24,C+=A|0,C^=K+1352,n=n>>>8|n<<24,n+=K|0,A=A<<a|A>>>G,A^=C,n^=Z+1352,K=K<<a|K>>>G,K^=n,Z++}return[A>>>24&255,A>>>16&255,A>>>8&255,A>>>0&255,C>>>24&255,C>>>16&255,C>>>8&255,C>>>0&255]},bF=function(a){return O.call(this,16,a)},C8=function(a,C,A,G,e,K,n,Z,V,k,y,v,d,h,N,x,B){if(n=I(212,A),n>=A.B)throw[T7,31];for(v=(h=(N=C,n),B=0,A).BH.length;N>0;)V=h>>3,k=h%8,x=A.j7[V],G=8-(k|0),K=G<N?G:N,a&&(e=h,d=A,d.L!=e>>6&&(d.L=e>>6,Z=Q(d,399),d.Q2=cI(3,d.L,d.F,29,[0,0,Z[1],Z[2]])),x^=A.Q2[V&v]),B|=(x>>8-(k|0)-(K|0)&(1<<K)-1)<<(N|0)-(K|0),N-=K,h+=K;return(y=B,f)(A,212,(n|0)+(C|0)),y},cw=function(a){{var C=9;while(C!=20)if(C==98)C=A.length===0?64:36;else{if(C==80)return eC(null,e,33);if(C==81)e+=encodeURIComponent(A[G])+a[G+1],C=97;else if(C==9)var A=f8.apply(1,(C=98,arguments));else if(C==88)C=6;else if(C==97)G++,C=6;else if(C==6)C=G<A.length?81:80;else if(C==36)var G=(C=88,0),e=a[0];else if(C==64)return eC(null,a[0],32)}}},O5=typeof Object.create=="function"?Object.create:function(a){function C(){}return new (C.prototype=a,C)},f=function(a,C,A){if(C==212||C==474){if(!""==!false)a.u[C]?a.u[C].concat(A):a.u[C]=p(15,A,a)}else{if(a.x6&&C!=399)return;C==263||C==177||C==431||C==140||C==260||C==440||C==143||C==374||C==131||C==348?a.u[C]||(a.u[C]=X(68,1,C,A,a,62)):a.u[C]=X(39,1,C,A,a,9)}if("D")C==399&&(a.F=C8(false,32,a),a.L=void 0)},s5=function(){return O.call(this,8)},Mp=function(a){return a.P?E4(a.G,a):C8(true,8,a)},tg=function(a,C,A,G,e){return gR.call(this,20,A,3,e,a,C,G)},Hw=function(a,C,A,G,e){e=typeof C;switch(!(e=="object")){case ![]==0:if(e=="function"&&typeof C.call=="undefined")return"object";break;case ![]==(null!=false):if(C){switch(!(C instanceof Array)){case NaN!==NaN:0;break;case ![]==(0==!!null):return"array";break}if(C instanceof Object)while("K"){return e;if(true!=[])break}G=Object.prototype.toString.call(C);while(G=="[object Window]"){return"object";if("D")break}while(G=="[object Array]"||typeof C.length==A&&typeof C.splice!="undefined"&&typeof C.propertyIsEnumerable!="undefined"&&!C.propertyIsEnumerable("splice")){return"array";if(-0===0)break}if(G=="[object Function]"||typeof C.call!="undefined"&&typeof C.propertyIsEnumerable!="undefined"&&!C.propertyIsEnumerable(a))while(true){return"function";if(true)break}}else return"null";break}return e},CR=function(a,C,A,G,e,K,n,Z){try{G=a[(2*(C&2)+~(C&2)-~(C|2))%3],a[C]=(e=(Z=a[C],n=a[((C|0)+1)%3],(Z|n)-(Z&n)-2*(~Z&n))-(G|0),K=C==1?G<<A:G>>>A,-(K|0)+(e|K)-(~e^K)+(~e|K))}catch(V){throw V;}},E5=function(a,C,A,G,e,K,n,Z,V,k){k=(Z=(((V=X(15,(e=G[Ic]||{},G)),e).R$=P(27,G),e).Z=[],G.X)==G?(n=Mp(G),-1+(~n^C)-A*(~n|C)):1,P)(a,G);{K=0;while(K<Z)e.Z.push(P(13,G)),K++}for(e.fQ=Q(G,k);Z--;)e.Z[Z]=Q(G,e.Z[Z]);return e.cH=U(G,V),e},Q=function(a,C,A){if(A=a.u[C],A===void 0)throw[T7,30,C];if(A.value)return A.create();return(A.create(C*4*C+-47*C+-73),A).prototype},dr=function(){return pD.call(this,6,40)},I=function(a,C,A){if(A=C.u[a],A===void 0)throw[T7,30,a];for(true;A.value;0!==-Number()){return A.create();if(true)break}return A.create(a*4*a+-47*a+-73),A.prototype},E,lF=function(a,C,A,G,e,K,n,Z,V){while(!G.O){G.Lu++;try{for(K=(V=(e=0,void 0),G).B;--A;)try{if((n=void 0,G).P)V=E4(G.P,G);else{if((e=I(212,G),e)>=K)break;V=U((n=X((R(G,474,e),23),G),G),n)}Zs(false,46,0,false,(V&&(Z=V[x4],-2*~(Z&2048)-C+~Z+(Z&-2049))?V(G,A):Bv(0,143,[T7,21,n],G),G),A)}catch(k){U(G,22)?Bv(22,143,k,G):R(G,22,k)}switch(!!A){case true:true+([]!=(!false==!""));break;case ![]==true:if(G.o$){lF(14,1,0xebd5d10eb8,(G.Lu--,G));return}Bv(0,143,[T7,33],G);break}}catch(k){try{Bv(22,143,k,G)}catch(y){c(a,":",G,y)}}if(G.Lu--,[])break}},iF=function(a,C,A,G,e,K,n,Z){return uF.call(this,12,a,26,C,A,G,e,K,n,Z)},L8=function(a,C,A,G,e,K){e.i3.length>K?Bv(a,C,[T7,36],e):(e.i3.push(e.u.slice()),e.u[A]=void 0,R(e,A,G))},uC=function(a,C){function A(){this.Y=this.e7=this.n=0}return[(C=(a=new (A.prototype.WU=(A.prototype.j$=function(){return SC.call(this,48)},function(G,e){return to.call(this,83,5,G,e)}),A),new A),function(G){a.WU(G),C.WU(G)}),function(G){return C=(G=[a.j$(),C.j$(),a.Y,C.Y],new A),G}]},ac=function(a,C,A,G,e,K,n,Z,V,k,y,v,d){if(V=a[0],V==Pw)!(K=G.g(a))||(K.PH=J.setTimeout(function(h){M(20,(M(68,G,(h=!G.A.length&&!G.o,K.CQ=true,K.Ou)),G),K.Ja),h&&c(57,true,G,true,true)},K.timeout),G.oE.push(K));else if(V==iC)G.Mu=25,G.V=true,G.Ni=G.nu()-a[2],G.g(a);else if(V==Zw){k=a[1][3];try{v=G.O||G.g(a)}catch(h){c(10,":",G,h),v=G.O}k((e=G.J(),v)),G.D+=G.J()-e}else if(V==dR)a[3]&&(G.l=true),a[4]&&(G.V=true),G.g(a);else if(V==Nl)G.l=true,G.g(a);else switch(!(V==h4)){case true:for(!""!=!(!false!=!"");V==K8;[]!=true){return Z=a[2],b(G,472,a[A]),R(G,249,Z),G.g(a);if(![]!=(false==Number()))break}V==x4?(G.g(a),G.l3=[],G.j7=[],G.u=null):V==n8&&J.document.readyState==="loading"&&(G.i=function(h,N){function x(B){{B=67;while(B!=68)B==65?(N=true,J.document.removeEventListener("DOMContentLoaded",x,vI),J.removeEventListener(C,x,vI),h(),B=68):B==67&&(B=N?68:65)}}(N=false,J.document).addEventListener("DOMContentLoaded",x,vI),J.addEventListener(C,x,vI)});break;case -0!==0:try{for(d=0;d<G.vU.length;d++)try{n=G.vU[d],n[0][n[1]](n[2])}catch(h){}}catch(h){}((0,a[1])(function(h,N){G.yn(h,true,N)},function(h){(M(48,G,(h=!G.A.length&&!G.o,[x4])),h)&&c(49,true,G,false,true)},(y=(G.vU=[],G.J()),function(h){return G.Vn(h)}),function(h,N,x){return G.KQ(h,N,x)}),G).D+=G.J()-y;break}},WI=function(a,C,A,G){for(A=(G=(C|0)-1,[]);G>=0;G--)A[(C|0)-1-(G|0)]=a>>G*8&255;while(false==![]){return A;if(true)break}},KR=function(a,C,A,G,e){switch(!(a.length==3)){case false==![]:[]!=(-0===0);break;case false:for(e=0;e<3;e++)C[e]+=a[e];A=[13,8,13,12,16,5,3,10,15];{G=0;while(G<9)C[3](C,G%3,A[G]),G++}break}},nR=function(a,C){return Vp.call(this,1,6,C,a)},aa=function(a,C){return M.call(this,7,a,C)},mx=function(a,C,A,G,e,K,n,Z,V,k,y,v){for(y=(C.mt=Ww(27,8,(C.Mi=((C.BH=C[Zw],C).su=yq,k4),C).H,{get:function(){return this.concat()}}),C.HH=Ra[C.H](C.mt,{value:{value:{}}}),v=[],0);y<364;y++)v[y]=String.fromCharCode(y);if((C.gG=(C.Ni=(C.z0=(C.Mu=25,C.J9=0,C.i=null,C.Q2=void 0,C.i3=[],C.P=void 0,!(C.x6=false,C.l3=(C.Lu=0,[]),C.L=(C.D=(C.LQ=[],(C.A=(C.F=void 0,(C.o=false,C).V=false,C.W=1,[]),C).Aa=30001,k=window.performance||{},C.HU=void 0,(C.ZU=Z,C).oE=[],0),void 0),1)),C.O=void 0,0),(C.sC=K,C).RE=0,C.Z9=0,C.X=C,C.dK=0,C.vU=(C.l=false,C.kj=(C.F7=[],V),[]),k.timeOrigin||(k.timing||{}).navigationStart||0),C).xN=function(d){return Np.call(this,7,d)},0===-0)C.B=0;if((C.uo=0,C).u=(C.dG=false,(C.aE=void 0,e)&&e.length==2&&(C.F7=e[1],C.LQ=e[0]),C.j7=[],C.G=void 0,[]),C.ml=0,n)try{C.aE=JSON.parse(n)}catch(d){C.aE={}}if((Y(14,function(d,h,N,x){f((h=X(21,(N=P(18,d),x=Mp(d),d)),d),h,Q(d,N)>>>x)},(Y(12,(Y(23,function(d,h,N,x,B,u){for(u=57;u!=62;)u==57?u=Zs(true,42,0,false,d,h)?62:47:u==41?u=d.X==d||x==d.xN&&B==d?87:62:u==87?(f(d,N.R$,x.apply(B,N.Z)),d.ml=d.J(),u=62):u==47&&(N=E5(7,1,2,d),x=N.cH,B=N.fQ,u=41)},C,(b(C,143,(R(C,(Y(23,(b(C,474,(b(C,(Y(12,function(d,h,N,x,B,u,S,L,q){for(q=39;q!=77;)q==85?q=N<S.length?70:96:q==39?(x=P(29,d),L=P(7,d),u=P(28,d),B=Q(d,x),S=Q(d,L),h="",N=0,q=8):q==96?(f(d,u,B[h]),q=77):q==70?(h+=String.fromCharCode(S[N]^121),q=98):q==98?(N++,q=85):q==8&&(q=85)},(Y(12,function(d,h,N,x,B,u,S,L,q){{q=52;while(q!=4)q==63?q=50:q==88?(S=(B=p(5,0,d),2*(S&B)-1+(S&~B)-(S|~B))%x,N.push(h[S]),q=10):q==10?q=50:q==52?(u=P(14,d),L=p(7,0,d),N=[],h=Q(d,265),S=0,x=h.length,q=63):q==50?q=L--?88:5:q==5&&(b(d,u,N),q=4)}},C,56),C),386),Y(12,function(d,h,N,x,B,u,S,L,q,T){{T=20;while(T!=22)T==20?(L=X(13,d),N=P(17,d),q=X(21,d),x=U(d,L),B=I(N,d),S=0,h="",T=55):T==86?T=S<x.length?61:60:T==61?(h+=String.fromCharCode((u=x[S],-122-~u+2*(~u&121))),T=29):T==29?(S++,T=86):T==60?(f(d,q,h in B|0),T=22):T==55&&(T=86)}},C,506),212),0),0)),function(d,h){R(d,(h=P(10,d),h),[])}),C,410),348),[2048]),[])),376)),function(d,h){L8(0,143,(h=I(P(23,d),d),212),h,d.X,104)}),C,434),C),280),NaN)!==NaN)Y(7,function(d,h,N,x,B,u){{u=1;while(u!=11)u==70?(h.push(Mp(d)),u=7):u==63?u=N<B?70:16:u==16?(R(d,x,h),u=11):u==1?(x=P(34,d),B=p(6,0,d),h=[],N=0,u=98):u==98?u=63:u==7&&(N++,u=63)}},C,151);Y(23,function(d,h,N,x){b((h=P(6,(N=P(13,(x=P(14,d),d)),d)),d),h,I(x,d)||U(d,N))},C,199);while(true)if(R(C,170,C),"H")break;if(R(C,263,[154,0,0]),{})Y(23,function(d,h,N){(N=P((h=P(11,d),20),d),R)(d,N,""+Q(d,h))},C,452);if((new Vq((Y(7,function(d,h,N,x,B){for(B=56;B!=13;)B==27?(N=P(19,d),x=X(13,d),f(d,x,function(u){return eval(u)}(os(I(N,d.X)))),B=13):B==56&&(B=Zs(true,47,0,false,d,h)?13:27)},(Y(14,function(d,h,N,x,B,u,S,L,q,T,D){{D=38;while(D!=56)D==38?(B=X(21,d),h=P(28,d),T=X(15,d),q=P(35,d),u=Q(d,q),x=I(B,d.X),S=Q(d,h),L=U(d,T),D=54):D==54?D=x!==0?44:56:D==44&&(N=X(48,2,1,d,u,1,L,x,S),x.addEventListener(S,N,vI),Q(d,462).push(function(){x.removeEventListener(S,N,vI)}),f(d,496,[x,S,N]),D=56)}},C,(Y(14,function(d,h,N,x,B){(B=(N=Q((h=(x=X(13,d),X)(13,d),d),x)!=0,I)(h,d),N)&&f(d,212,B)},(R((f(C,140,(Y(12,function(d,h,N,x,B,u,S,L,q,T,D,vw,F,z,W){{W=97;while(W!=41)if(W==14)W=18;else if(W==10)q=0,T=z.length,F=F>0?F:1,W=14;else if(W==29){for(vw in u=[],z)u.push(vw);W=(z=u,83)}else W==38?(q+=F,W=18):W==97?W=Zs(true,56,0,true,d,h)?41:24:W==60?W=Hw("call",z,"number")=="object"?29:83:W==67?(S(z.slice(q,(q|0)+(F|0)),D),W=38):W==24?(L=P(15,d),B=X(14,d),N=P(26,d),x=X(22,d),S=Q(d,B),F=I(N,d),z=Q(d,L),D=U(d,x),W=60):W==83?W=d.X==d?10:41:W==18&&(W=q<T?67:41)}},(Y((Y(7,function(d){Mh(1,d)},C,(Y(13,function(d,h,N,x,B,u){b(d,(N=(B=Q((u=P(19,(h=P(17,(x=P(36,d),d)),d)),d),h),I(x,d)),u),N in B|0)},(Y(13,function(d,h,N,x,B,u,S,L,q){for(q=58;q!=99;)q==58?(B=P(15,d),u=p(3,0,d),S="",N=I(265,d),L=N.length,h=0,q=17):q==0?(b(d,B,S),q=99):q==17?q=84:q==84?q=u--?1:0:q==26?q=84:q==1&&(h=(x=p(3,0,d),-2*~(h&x)+-3+(h&~x)-(h|~x))%L,S+=v[N[h]],q=26)},C,(b(C,431,[]),157)),C),332),269)),12),function(d,h,N,x,B,u,S,L){(h=U(d,(B=(x=(N=P(12,(S=P(8,(u=P(35,d),d)),d)),X)(21,d),I(N,d)),L=I(x,d),S)),R)(d,u,X(49,2,1,d,B,L,h))},C,174),C),50),dF)(4)),C),462,[]),C),41),284)),C.WH=0,C),377),"Submit"))).dispose(),"p")R(C,496,0);Y(13,function(d,h,N,x,B,u,S){{S=6;while(S!=70)if(S==6){for(x in N=Q(d,(u=Mp(d),184)),delete N[u],N)if(x!=="t"&&Object.hasOwn(N,x))return;S=83,h=0}else S==83?S=33:S==77?(B=d.oE[h],S=12):S==12?S=B.CQ?88:76:S==76?(J.clearTimeout(B.PH),M(64,d,B.Ou),M(52,d,B.Ja),S=88):S==98?(d.oE.length=0,S=70):S==88?(h++,S=33):S==33&&(S=h<d.oE.length?77:98)}},(R(C,(Y(23,(Y(14,(f((Y(12,function(d){Mh(4,d)},C,(b(C,131,(Y(13,function(d,h,N,x,B,u){b(d,(B=Q((u=(x=X((h=P(26,(N=X(22,d),d)),23),d),Q)(d,N),d),h),x),+(u==B))},C,(Y(23,function(){},(R(C,(Y(14,(Y(14,function(d,h,N,x,B,u,S,L,q,T){for(T=15;T!=47;)T==2?(x=E5(7,1,2,d.X),L=x.fQ,u=x.Z,B=x.cH,q=x.R$,N=u.length,S=N==0?new L[B]:N==1?new L[B](u[0]):N==2?new L[B](u[0],u[1]):N==3?new L[B](u[0],u[1],u[2]):N==4?new L[B](u[0],u[1],u[2],u[3]):2(),f(d,q,S),T=47):T==15&&(T=Zs(true,43,0,false,d,h)?47:2)},C,311),function(d){wr(3,d)}),C,46),184),{}),C),128),21)),dF(4))),C.bo=0,238)),C),260,[]),function(d,h,N,x,B,u){f((h=(x=Q((B=P(18,(u=P(37,(N=P(8,d),d)),d)),d),u),Q(d,N)),d),B,h[x])}),C,102),function(d,h,N,x,B){{B=98;while(B!=83)B==80?B=34:B==49?(h--,B=34):B==86?(h=Mp(d),B=80):B==34?B=h>0?6:14:B==14?(x[260]=d.u[260],x[348]=d.u[348],d.u=x,B=83):B==6?(N=P(29,d),x[N]=d.u[N],B=49):B==32?B=x?86:16:B==16?(b(d,212,d.B),B=83):B==98&&(x=d.i3.pop(),B=32)}}),C,219),Y(13,function(d,h,N,x,B){b(d,(h=U(d,(N=(B=P(9,(x=X(23,d),d)),U(d,B)),x)),B),N+h)},C,295),R(C,249,{}),181),0),C),224);while("V")if(Y(7,function(d,h,N){h=(N=P(16,d),U(d.X,N)),h[0].removeEventListener(h[1],h[2],vI)},C,466),![""]==0)break;c(48,true,C,true,(M((M(48,((Y(14,function(d,h,N,x,B){(N=Hw("call",(x=Q((h=X((B=P(10,d),15),d),d),B),x),"number"),f)(d,h,N)},C,(b(C,374,(Y(7,function(d,h,N,x,B,u,S,L,q,T,D,vw,F,z,W,Tf,m,oa,l){{l=93;while(l!=25)l==6?l=43:l==5?(T++,l=77):l==83?(S=0,l=6):l==23?(vw.push(U(d,P(12,d))),l=39):l==53?l=77:l==27?(m++,l=80):l==44?(S++,l=43):l==33?(Y(7,function(g,e9,nD,ho,t,H){for(H=94;H!=5;)H==4?(nD.push(t),H=72):H==47?H=t>=ho.length?73:23:H==1?(g.P=p(17,vw.slice(),g),g.G=p(16,nD,g),H=5):H==19?H=e9<z?64:1:H==72?(e9++,H=19):H==73?(ho.push(X(22,g)),H=34):H==35?H=47:H==43?H=19:H==23?(t=ho[t],H=4):H==64?(t=Tf[e9],H=0):H==0?H=W[e9]?4:35:H==34?H=47:H==94&&(e9=0,ho=[],nD=[],H=43)},d,B),l=25):l==39?l=47:l==40?(L=(-~N+~(N|1)+(N&-2)).toString(2).length,m=0,Tf=[],l=66):l==4?(q=h,vw=[],l=67):l==80?l=m<z?21:83:l==66?l=80:l==47?l=q--?23:33:l==43?l=S<z?73:4:l==67?l=47:l==70?(u=oa(1),W.push(u),N+=u?0:1,l=5):l==77?l=T<z?70:40:l==21?(W[m]||(Tf[m]=oa(L)),l=27):l==93?(oa=function(g,e9){for(;F<g;)x|=Mp(d)<<F,F+=8;return F-=g,x>>=(e9=x&(1<<g)-1,g),e9},B=X(14,d),F=x=0,h=(D=oa(3),-(D|1)+3*(D&1)+2*(D^1)),z=oa(5),W=[],T=N=0,l=53):l==73&&(W[S]&&(Tf[S]=P(27,d)),l=44)}},(b(C,(Y((Y(13,function(d){p(24,8,4,d)},C,(f((R(C,(Y(13,function(d){wr(4,d)},C,(C.cI=(b(C,22,234),0),441)),177),dF(4)),C),440,[]),36)),7),function(d,h,N,x,B,u,S){{S=14;while(S!=55)S==0?(d.L=void 0,S=98):S==86?(N=Q(d,h),B=Q(d,u),U(d,x)[B]=N,S=30):S==32?S=d.X==d?86:55:S==98?S=B==2?72:55:S==14?(x=P(6,d),u=X(14,d),h=P(11,d),S=32):S==72?(d.F=C8(false,32,d),d.L=void 0,S=55):S==30&&(S=x==399?0:55)}},C,308),74),J),C),354),[0,0,0])),186)),G)||M(16,C,[n8]),C),[Nl,A]),52),C,[h4,a]),true))},dF=function(a,C,A){for(A=9;A!=51;)if(A==94)A=a--?82:81;else if(A==54)A=94;else{if(A==81)return C;A==82?(C.push(Math.random()*255|0),A=54):A==37?A=94:A==9&&(C=[],A=37)}},BI=function(a){return yp.call(this,42,a)},G7=function(a){return gR.call(this,20,a,9)},Qp=function(){return Y.call(this,16)},xt=function(){return CD.call(this,20,16,26)},Vq=function(a,C,A){return uF.call(this,12,a,21,C,A)},vv=function(a,C,A,G,e){return Ia.call(this,"class",86,a,C,A,G,e)},e1=function(a,C,A,G,e){return SC.call(this,36,a,C,A,G,e)},$t=function(a,C,A,G,e,K,n,Z,V,k){if(A.X==A)for(V=Q(A,G),G==177||G==131||G==140?(n=function(y,v,d,h,N,x,B,u){for(B=(u=72,2);;)try{if(u==76)break;else if(u==72)d=V.length,v=(d|0)-C>>3,u=32;else if(u==22)B=32,V.gK=cI(3,X(24,C,2*(N&C)-~N+-5+2*(~N&C),V),X(26,C,N,V),29,h),u=41;else{if(u==3)throw B=2,x;u==32?u=V.wK!=v?88:41:u==88?(h=[0,0,Z[1],Z[2]],N=(v<<3)-C,V.wK=v,u=22):u==41&&(V.push(V.gK[d&7]^y),u=76)}}catch(S){if(B==2)throw S;B==32&&(x=S,u=3)}},Z=U(A,374)):n=function(y){V.push(y)},e&&n(255-~(e&255)+-256),K=0,k=a.length;K<k;K++)n(a[K])},Bv=function(a,C,A,G,e,K,n,Z,V,k,y,v,d){switch(!(!G.x6&&(v=void 0,A&&A[0]===T7&&(a=A[1],v=A[2],A=void 0),k=U(G,260),k.length==0&&(n=U(G,474)>>3,k.push(a,n>>8&255,n&255),v!=void 0&&k.push(v&255)),V="",A&&(A.message&&(V+=A.message),A.stack&&(V+=":"+A.stack)),K=U(G,348),K[0]>3))){case ![]==(!""!=!false):undefined;break;case NaN===NaN:G.X=(d=(V=Jg((K[0]-=(V=V.slice(0,(Z=K[0],-(Z&3)-1-2*~Z+~(Z|3))),(V.length|0)+3),V),224),G.X),G);try{G.z0?(e=(e=I(440,G))&&e[e.length-1]||95,(y=I(C,G))&&y[y.length-1]==e||$t([e&255],4,G,C)):Yt(G,440,[95]),$t(WI(V.length,2).concat(V),4,G,177,51)}finally{G.X=d}break}},iZ=function(a,C,A,G,e,K,n,Z,V,k){return uZ.call(this,48,C,a,A,G,e,K,n,Z,V,k)};switch(!(typeof Object.setPrototypeOf=="function")){case !(!(!""!=!false)!=!"")==!"":var rF;a:{var ql={},S1={a:true};try{rF=ql.a,ql.__proto__=S1;break a}catch(a){}rF=false}PI=rF?function(a,C,A){{A=74;while(A!=85){if(A==77)return a;if(A==74)a.__proto__=C,A=98;else if(A==98)A=a.__proto__!==C?53:77;else if(A==53)throw new TypeError(a+" is not extensible");}}}:null;break;case false:PI=Object.setPrototypeOf;break}var Qq=PI,J=this||self,Vf="closure_uid_"+(Math.random()*1E9>>>0),LD,Nh=0,wR=function(a,C,A,G,e,K){while(![undefined]==Number())if(G=46,{})break;{K=40;while([])try{if(G==88)break;else if(G==46)G=J.addEventListener&&Object.defineProperty?41:50;else if(G==41)C=false,A=Object.defineProperty({},"passive",{get:function(){C=true}}),G=26;else{if(G==39)return K=40,C;if(G==26)K=51,a=function(){},J.addEventListener("test",a,A),J.removeEventListener("test",a,A),G=39;else if(G==93)K=40,G=39;else if(G==50)return false}}catch(n){if(K==40)throw n;K==51&&(e=n,G=93)}}}(),s4="closure_listenable_"+((((O(59,((dr.prototype.u3=!(dr.prototype.dispose=(kt.prototype.stopPropagation=function(){this.k6=true},kt.prototype.preventDefault=function(){this.defaultPrevented=true},function(a){for(a=94;a!=92;)a==94?a=this.u3?92:65:a==65&&(this.u3=true,this.h(),a=92)}),1),dr.prototype)[dr.prototype.h=function(a){{a=53;while(a!=2)a==19?a=this.FD.length?66:2:a==66?(this.FD.shift()(),a=93):a==53?a=this.FD?33:2:a==93?a=19:a==33&&(a=19)}},Symbol.dispose]=function(){this.dispose()},2),aa,kt),aa.prototype).init=function(a,C,A,G,e,K){for(K=22;K!=62;)K==22?(G=this.type=a.type,e=a.changedTouches&&a.changedTouches.length?a.changedTouches[0]:null,this.target=a.target||a.srcElement,this.currentTarget=C,A=a.relatedTarget,K=3):K==55?(this.relatedTarget=A,K=64):K==53?(this.offsetX=a.offsetX,this.offsetY=a.offsetY,this.clientX=a.clientX!==void 0?a.clientX:a.pageX,this.clientY=a.clientY!==void 0?a.clientY:a.pageY,this.screenX=a.screenX||0,this.screenY=a.screenY||0,K=43):K==44?(A=a.fromElement,K=55):K==3?K=A?55:36:K==12?K=G=="mouseout"?34:55:K==43?(this.button=a.button,this.keyCode=a.keyCode||0,this.key=a.key||"",this.charCode=a.charCode||(G=="keypress"?a.keyCode:0),this.ctrlKey=a.ctrlKey,this.altKey=a.altKey,this.shiftKey=a.shiftKey,this.metaKey=a.metaKey,this.pointerId=a.pointerId||0,this.pointerType=a.pointerType,this.state=a.state,this.timeStamp=a.timeStamp,this.Qn=a,a.defaultPrevented&&aa.R.preventDefault.call(this),K=62):K==36?K=G=="mouseover"?44:12:K==96?(this.clientX=e.clientX!==void 0?e.clientX:e.pageX,this.clientY=e.clientY!==void 0?e.clientY:e.pageY,this.screenX=e.screenX||0,this.screenY=e.screenY||0,K=43):K==34?(A=a.toElement,K=55):K==64&&(K=e?96:53)},aa.prototype).stopPropagation=function(){aa.R.stopPropagation.call(this),this.Qn.stopPropagation?this.Qn.stopPropagation():this.Qn.cancelBubble=true},aa.prototype).preventDefault=function(a){(a=(aa.R.preventDefault.call(this),this.Qn),a).preventDefault?a.preventDefault():a.returnValue=false},Math.random()*1E6|0),mO=0,Fq="constructor hasOwnProperty isPrototypeOf propertyIsEnumerable toLocaleString toString valueOf".split(" "),XP="closure_lm_"+(((zf.prototype.hasListener=function(a,C,A,G,e){return yp(24,(e=(G=a!==(A=C!==void 0,void 0))?a.toString():"",false),true,this.j,function(K,n,Z){for(Z=67;Z!=37;)if(Z==20)Z=33;else if(Z==59)++n,Z=33;else{if(Z==40)return true;if(Z==25)Z=G&&K[n].type!=e||A&&K[n].capture!=C?59:40;else if(Z==33)Z=n<K.length?25:14;else{if(Z==14)return false;Z==67&&(n=0,Z=20)}}})},zf.prototype.remove=function(a,C,A,G,e,K,n,Z){{Z=93;while(Z!=82)if(Z==47)Z=n.length==0?5:3;else if(Z==81)n=this.j[K],e=kD(0,34,A,G,C,n),Z=38;else if(Z==36)Z=K in this.j?81:73;else if(Z==5)delete this.j[K],this.fu--,Z=3;else{if(Z==43)return false;if(Z==80)O(69,null,n[e]),Array.prototype.splice.call(n,e,1),Z=47;else{if(Z==3)return true;if(Z==73)return false;Z==38?Z=e>-1?80:43:Z==93&&(K=a.toString(),Z=36)}}}},zf.prototype).add=(zf.prototype.BU=function(a,C,A,G,e,K){K=this.j[a.toString()];while({})if(e=-1,[]!=true)break;return(K&&(e=kD(0,33,C,A,G,K)),e>-1)?K[e]:null},function(a,C,A,G,e,K,n,Z,V,k){{k=98;while(k!=13)if(k==24)k=n>-1?93:67;else if(k==6)V=this.j[K]=[],this.fu++,k=56;else if(k==78)k=A?55:42;else if(k==93)Z=V[n],k=78;else if(k==98)K=a.toString(),V=this.j[K],k=50;else if(k==42)Z.pu=false,k=55;else if(k==67)Z=new tg(K,C,e,!!G,this.src),Z.pu=A,V.push(Z),k=55;else{if(k==55)return Z;k==56?(n=kD(0,32,G,e,C,V),k=24):k==50&&(k=V?56:6)}}}),Math).random()*1E6|0),j9={},Ds=0,fD="__closure_events_fn_"+(Math.random()*1E9>>>0);(((((E=((O(25,2,Qp,dr),Qp).prototype[s4]=true,Qp).prototype,E).Y6=function(a){this.IE=a},E).addEventListener=function(a,C,A,G){O4(88,3,"object",0,a,A,this,C,G)},E.removeEventListener=function(a,C,A,G){Np(32,"object",0,G,this,a,A,C)},E.dispatchEvent=function(a,C,A,G,e,K,n,Z,V,k,y,v){{v=96;while(v!=87)if(v==94)v=V?14:71;else if(v==72)Z.target=Z.target||A,v=17;else if(v==5)v=C?84:10;else if(v==39)v=!Z.k6&&n<V.length?27:71;else if(v==41)v=93;else if(v==70)v=V?58:57;else if(v==89)v=39;else if(v==14)n=0,v=89;else if(v==96)C=this.IE,v=23;else if(v==57)v=Z.k6?94:85;else if(v==49)e=Z,Z=new kt(K,A),U5(Z,e),v=17;else if(v==30)k=Z.currentTarget=V[n],G=yp(14,1,k,Z,K,true)&&G,v=42;else if(v==10)Z=a,K=Z.type||Z,V=y,A=this.I$,v=51;else if(v==17)G=true,v=70;else if(v==97)C=C.IE,v=5;else if(v==54)v=5;else if(v==84)y.push(C),v=97;else if(v==23)v=C?95:10;else if(v==85)k=Z.currentTarget=A,G=yp(6,1,k,Z,K,true)&&G,Z.k6||(G=yp(7,1,k,Z,K,false)&&G),v=94;else if(v==42)n--,v=93;else{if(v==71)return G;v==51?v=typeof Z==="string"?52:46:v==95?(y=[],v=54):v==27?(k=Z.currentTarget=V[n],G=yp(10,1,k,Z,K,false)&&G,v=73):v==93?v=!Z.k6&&n>=0?30:57:v==73?(n++,v=39):v==58?(n=V.length-1,v=41):v==46?v=Z instanceof kt?72:49:v==52&&(Z=new kt(Z,A),v=17)}}},E).h=function(){(Qp.R.h.call(this),this).C&&bZ(5,this.C,0,null,6),this.IE=null},E).BU=function(a,C,A,G){return this.C.BU(String(a),C,A,G)},E).hasListener=function(a,C){while({}){return this.C.hasListener(a!==void 0?String(a):void 0,C);if(true)break}};var $D,Ag,LR=(G7.prototype.toString=function(){return this.pQ+""},Error),Xq={iS:0,S$:1,YN:2,0:"FORMATTED_HTML_CONTENT",1:"EMBEDDED_INTERNAL_CONTENT",2:"EMBEDDED_TRUSTED_EXTERNAL_CONTENT"};(rr.prototype=O5(LR.prototype),rr.prototype).constructor=rr;switch(!Qq){case true:for(var lC in LR)if(lC!="prototype")if(Object.defineProperties){var Rs=Object.getOwnPropertyDescriptor(LR,lC);Rs&&Object.defineProperty(rr,lC,Rs)}else rr[lC]=LR[lC];break;case false:Qq(rr,LR);break}((((O(27,2,BI,(((uF(12,((((E=("ARTICLE SECTION NAV ASIDE H1 H2 H3 H4 H5 H6 HEADER FOOTER ADDRESS P HR PRE BLOCKQUOTE OL UL LH LI DL DT DD FIGURE FIGCAPTION MAIN DIV EM STRONG SMALL S CITE Q DFN ABBR RUBY RB RT RTC RP DATA TIME CODE VAR SAMP KBD SUB SUP I B U MARK BDI BDO SPAN BR WBR NOBR INS DEL PICTURE PARAM TRACK MAP TABLE CAPTION COLGROUP COL TBODY THEAD TFOOT TR TD TH SELECT DATALIST OPTGROUP OPTION OUTPUT PROGRESS METER FIELDSET LEGEND DETAILS SUMMARY MENU DIALOG SLOT CANVAS FONT CENTER ACRONYM BASEFONT BIG DIR HGROUP STRIKE TT".split((rr.R=LR.prototype," ")).concat(["BUTTON","INPUT"]),lZ).prototype,E).S=function(a){return typeof a==="string"?this.Cu.getElementById(a):a},E.getElementsByTagName=function(a,C){return(C||this.Cu).getElementsByTagName(String(a))},E).createElement=function(a,C,A){return((C=(A=this.Cu,String(a)),A.contentType==="application/xhtml+xml")&&(C=C.toLowerCase()),A).createElement(C)},E.createTextNode=function(a){return this.Cu.createTextNode(String(a))},E.appendChild=function(a,C){a.appendChild(C)},E.append=function(a,C){SC(20,"string","","array",0,arguments,a.nodeType==9?a:a.ownerDocument||a.document,a)},E.canHaveChildren=function(a,C){for(C=59;C!=49;){if(C==98)return false;if(C==59)C=a.nodeType!=1?98:28;else if(C==28){switch(a.tagName){case "APPLET":case "AREA":case "BASE":case "BR":case "COL":case "COMMAND":case "EMBED":case "FRAME":case "HR":case "IMG":case "INPUT":case "IFRAME":case "ISINDEX":case "KEYGEN":case "LINK":case "NOFRAMES":case "NOSCRIPT":case "META":case "OBJECT":case "PARAM":case "SCRIPT":case "SOURCE":case "STYLE":case "TRACK":case "WBR":return false}return true}}},E.removeNode=bF,E).contains=function(a,C,A,G){{G=74;while(G!=36){if(G==82)return false;if(G==29)G=typeof a.compareDocumentPosition!="undefined"?12:18;else if(G==74)G=a&&C?84:82;else if(G==84)G=a.contains&&C.nodeType==1?7:29;else{if(G==12)return a==C||!!(A=a.compareDocumentPosition(C),15-(A^16)-(~A|16));if(G==18)G=3;else if(G==3)G=C&&a!=C?43:64;else if(G==43)C=C.parentNode,G=5;else{if(G==64)return C==a;if(G==5)G=3;else if(G==7)return a==C||a.contains(C)}}}}},DE),8),DE).prototype.nQ="",DE).prototype.wG=0,Qp)),E=BI.prototype,E).GA=DE.Ku(),E).S=function(){return this.U},E.getParent=function(){return this.XD},E.cU=function(){uZ(32,this,function(a){a.D9&&a.cU()});while([])if((this.b3&&bZ(5,this.b3,0,null,12),-Number())===Number())break;this.D9=false},E).h=function(a){{a=14;while(a!=96)a==98?(uZ(34,this,function(C){C.dispose()}),!this.Uu&&this.U&&bF(this.U),this.U=this.qu=this.XD=this.OC=null,BI.R.h.call(this),a=96):a==35?(this.b3.dispose(),delete this.b3,a=98):a==7?a=this.b3?35:98:a==14&&(this.D9&&this.cU(),a=7)}},E).Y6=function(a,C){{C=22;while(C!=96){if(C==17)throw Error("Method not supported");C==22?C=this.XD&&this.XD!=a?17:28:C==28&&(BI.R.Y6.call(this,a),C=96)}}},E.removeChild=function(a,C,A,G,e,K,n,Z,V,k,y,v){{v=88;while(v!=12)if(v==85)v=this.qu&&A?17:47;else if(v==22)a.cU(),a.U&&bF(a.U),v=21;else if(v==51)K=a,v=99;else if(v==87)v=e==null?29:18;else if(v==21)e=a,v=87;else if(v==6)v=A&&a?16:91;else if(v==18)e.XD=null,BI.R.Y6.call(e,null),v=91;else{if(v==54)throw Error("Child is not in parent component");if(v==88)v=a?76:91;else if(v==2)v=C?22:21;else if(v==80)K=V,v=99;else if(v==16)y=this.qu,A in y&&delete y[A],Y(40,1,a,this.OC),v=2;else if(v==47)Z=null,v=24;else if(v==91)v=a?20:54;else if(v==99)A=K,v=85;else if(v==24)a=Z,v=6;else{if(v==29)throw Error("Unable to set parent component");if(v==37)v=(V=a.kN)?80:92;else if(v==92)k=a.GA,n=k.nQ+":"+(k.wG++).toString(36),V=a.kN=n,v=80;else if(v==17)G=this.qu,Z=(G!==null&&A in G?G[A]:void 0)||null,v=24;else if(v==76)v=typeof a==="string"?51:37;else if(v==20)return a}}}};var TU,cv={button:"pressed",checkbox:(uF(12,Qf,10),"checked"),menuitem:"selected",menuitemcheckbox:"checked",menuitemradio:"checked",radio:"checked",tab:"selected",treeitem:"selected"},hg=(uF((O(56,2,((E=Qf.prototype,E.PU=function(a,C,A,G){(G=a.S?a.S():a)&&(A?vv:nR)(G,[C])},E.I=function(a,C,A,G,e,K,n){for(n=2;n!=86;)n==60?n=this.uS?10:19:n==10?((K=this.uS[C])&&this.PU(a,K,A),this.G0(e,C,A),n=86):n==9?n=e?60:86:n==19?(G=this.bS(),G.replace(/\\xa0|\\s/g," "),this.uS={1:G+"-disabled",2:G+"-hover",4:G+"-active",8:G+"-selected",16:G+"-checked",32:G+"-focused",64:G+"-open"},n=10):n==2&&(e=a.S(),n=9)},E.T0=function(a){return a.S()},E).G0=function(a,C,A,G,e,K,n,Z){((TU||(TU={1:"disabled",8:"selected",16:"checked",64:"expanded"}),Z=TU[C],G=a.getAttribute("role")||null)?(n=cv[G]||Z,K=Z=="checked"||Z=="selected"?n:Z):K=Z,e=K)&&FP("false",e,A,a,"off",6)},E.bS=function(){return"goog-control"},E.S7=function(a,C,A,G,e,K,n,Z,V){for(Z=(n=3,80);;)try{if(n==31)break;else n==25?(e=G,n=95):n==4?(e.tabIndex=-1,e.removeAttribute("tabIndex"),n=31):n==50?(Z=15,G.blur(),n=30):n==84?n=K!=C?25:31:n==24?(e.tabIndex=0,n=31):n==30?(Z=80,n=97):n==97?n=a.K&32?5:58:n==58?n=(K=G.hasAttribute("tabindex"))?36:84:n==8?n=!C&&a.K&32?50:58:n==36?(A=G.tabIndex,K=typeof A==="number"&&A>=0&&A<32768,n=84):n==5?(Vp(1,9,0,2,a,4)&&a.setActive(false),Vp(1,11,0,2,a,32)&&Bw(29,8,21,a,32,false)&&a.I(32,false),n=58):n==14?(Z=80,n=30):n==95?n=C?24:4:n==3&&(n=a.rK&32&&(G=a.T0())?8:31)}catch(k){if(Z==80)throw k;Z==15&&(V=k,n=14)}},s5),Qf),12),s5,6),{});(E=(O(57,2,((s5.prototype.bS=function(){return"goog-button"},s5.prototype).G0=function(a,C,A){switch(C){case 8:case 16:FP("false","pressed",A,a,"off",5);break;default:case 64:case 1:s5.R.G0.call(this,a,C,A)}},iF),BI),iF.prototype),E).rG=255;while(!![])if(E.T0=function(){return this.T.T0(this)},true)break;E.isEnabled=((E.I=function(a,C,A,G,e,K){{K=78;while(K!=36)K==31?(this.setActive(false),Bw(29,8,13,this,2,false)&&this.I(2,false),K=59):K==40?(e=!C,G=this.getParent(),K=20):K==51?(this.T.I(this,a,C),this.K=C?this.K|a:this.K&~a,K=36):K==59?(this.isVisible()&&this.T.S7(this,e),this.I(1,!e,true),K=36):K==22?K=this.rK&a&&C!=!!(this.K&a)?51:36:K==78?K=A||a!=1?22:40:K==35?K=e?59:31:K==20&&(K=G&&typeof G.isEnabled=="function"&&!G.isEnabled()||!Bw(29,8,5,this,1,!e)?36:35)}},E.h=function(a){{a=35;while(a!=22)a==86?(this.Nu.dispose(),delete this.Nu,a=11):a==47?a=this.Nu?86:11:a==11?(delete this.T,this.N=null,a=22):a==35&&(iF.R.h.call(this),a=47)}},E.N=null,(E.isVisible=(E.setActive=function(a){Bw(29,8,12,this,4,a)&&this.I(4,a)},function(){while("S"){return this.DU;if("Z")break}}),E).PU=(E.getState=function(){return this.K},function(a,C,A){{A=98;while(A!=34)A==52?A=a?64:34:A==70?A=this.N.length==0?5:25:A==98?A=C?52:54:A==54?A=a&&this.N&&Y(43,1,a,this.N)?70:34:A==5?(this.N=null,A=25):A==64?(this.N?yp(37,0,a,this.N)||this.N.push(a):this.N=[a],this.T.PU(this,a,true),A=34):A==25&&(this.T.PU(this,a,false),A=34)}}),E.K=(E.isActive=function(){return!!(this.K&4)},0),E.cU=(E.DU=true,function(){iF.R.cU.call(this),this.Nu&&this.Nu.detach(),this.isVisible()&&this.isEnabled()&&this.T.S7(this,false)}),E.rK=39,E).TA=0,function(){return!(this.K&1)});while(typeof iF!=="function"){throw Error("Invalid component class "+iF);if(!(!!null!=![])==!"")break}if(typeof Qf!=="function")throw Error("Invalid renderer class "+Qf);var Y4=O4(88,49,iF),KD=(kD(function(){return new Vq(null)},17,((O((((uF(12,xt,(O(26,(kD(function(){return new iF(null)},(hg[Y4]=Qf,18),"goog-control"),2),xt,s5),9)),xt.prototype.I=function(a,C,A,G,e){{e=49;while(e!=60)e==49?(xt.R.I.call(this,a,C,A),G=a.S(),e=62):e==62?e=G&&C==1?55:60:e==55&&(G.disabled=A,e=60)}},xt.prototype).G0=function(){},xt).prototype.S7=function(){},58),2,Vq,iF),Vq).prototype.h=function(){delete (Vq.R.h.call(this),delete this.JH,this).fV},"goog-button")),J.requestIdleCallback?function(a){requestIdleCallback(function(){a()},{timeout:4})}:J.setImmediate?function(a){while(![""]==0)if(setImmediate(a),true)break}:function(a){setTimeout(a,0)}),vI={passive:true,capture:true},xD,YD=O(3,["https://www.google.com/js/bg/",""]),Ic=String.fromCharCode(105,110,116,101,103,67,104,101,99,107,66,121,112,97,115,115),K8=[],T7={},h4=[],x4=[],dR=(gr.prototype.jl=(gr.prototype.X8=void 0,void 0),gr.prototype.o$=false,[]),Pw=[],Zw=[],n8=(gr.prototype.EC="toString",[]),iC=[],Nl=[],os=function(a,C){return(C=gR(20,"error",32))&&a.eval(C.createScript("1"))===1?function(A){return C.createScript(A)}:function(A){return""+A}}(J),Wv=function(a){return(a=gR(20,"error",10))?function(C){return a.createScriptURL(C)}:function(C){return C}}(),Ml=function(a){return(a=gR(20,"error",26))?function(C){return a.createHTML(C)}:function(C){return C}}(),yf=(((((((qh,function(){})(dF),function(){})(CR),KR,function(){})(z7),uC,e1,HI,function(){})(Wv),function(){})(Ml),gr.prototype.H="create",gr.prototype).Eu=0,void 0);if(NaN!==NaN)E=gr.prototype;var Ra=(E.zA=(E.e$=function(a,C,A,G,e,K){return bZ.call(this,5,A,C,a,7,G,e,K)},E.J=(window.performance||{}).now?function(){while("l"){return this.gG+window.performance.now();if(true)break}}:function(){return+new Date},E.QE=(E.ta=0,function(a,C,A,G,e,K){return Gf.call(this,63,11,A,a,C,G,e,K)}),E.V2=function(){return Ia.call(this,"class",41)},function(a,C,A,G,e,K,n,Z,V,k){return uZ.call(this,3,C,a,A,G,e,K,n,Z,V,k)}),E.yn=function(a,C,A,G,e){return p.call(this,56,a,C,A,G,e)},E.nu=function(){return M.call(this,35)},T7).constructor,k4=((E=gr.prototype,E).g=function(a,C){return a=(yf=function(){return C==a?-73:-114},C={},{}),function(A,G,e,K,n,Z,V,k,y,v,d,h,N,x,B,u,S,L,q,T,D,vw,F,z,W,Tf,m,oa,l,g,e9,nD,ho,t,H,as,jC,A4,w,Pv,Ao,r,GU,j1,Jo){for(j1=(Ao=undefined,r=(w=77,47),false);;)try{if(r==5)break;else if(r==21)w=97,lF(14,1,30001,this),r=33;else if(r==46)Jo={Ou:n,Ja:vw,timeout:q,CQ:false,PH:0},Ao=20,r=33;else if(r==99)this.A.push(vw),this.A.push(n),Ao=5,r=33;else if(r==61)r=32;else if(r==75)w=97,h=dF(2).concat(I(263,this)),h[1]=(ho=h[0],(ho|60)+~ho-(~ho|60)),h[3]=(V=h[1],F=jC[0],(V|0)-(F|0)+2*(~V&F)),h[4]=(v=h[1],N=jC[1],-(v|0)-1-~(v|N)+(v&~N)),Z=this.ha(h),r=55;else if(r==8)Z="*"+Z,r=73;else if(r==4)w=97,n=[iC,A,this.nu()],vw=[Zw,A],r=93;else if(r==16)r=t.length>4?66:23;else if(r==26)x=A[1],x[1].push(U(this,143).length,I(177,this).length,U(this,440).length,I(131,this).length,Q(this,263).length,I(431,this).length,Q(this,140).length,U(this,348)[0]),b(this,249,x[2]),this.u[480]&&Rc(249,104,30001,this,I(480,this)),r=33;else if(r==94)Jo=Rc(249,104,30001,this,A[1]),Ao=29,r=33;else if(r==73)as=Z,Q(this,143).length=m.shift(),U(this,177).length=m.shift(),Q(this,440).length=m.shift(),U(this,131).length=m.shift(),Q(this,263).length=m.shift(),I(431,this).length=m.shift(),I(140,this).length=m.shift(),I(348,this)[0]=m.shift(),Jo=as,Ao=72,r=33;else{if(r==20)return Jo;if(r==32)r=S.done?27:86;else if(r==65)this.A.push(vw),this.A.push(n),Ao=5,r=33;else if(r==36)r=Ao!==undefined?33:75;else if(r==0){if(D=(B=I(462,this),typeof Symbol!="undefined"&&Symbol.iterator&&B[Symbol.iterator]))H=D.call(B);else if(typeof B.length=="number")H={next:Ww(27,21,0,B)};else throw Error(String(B)+" is not an iterable or ArrayLike");e9=H,S=e9.next(),r=61}else if(r==13)r=nD==Zw?92:44;else if(r==30)w=97,nD=A[0],r=12;else if(r==55)r=Z?8:97;else if(r==15)w=42,y=U(this,184)||{},q=y.t|0,r=4;else if(r==78)e=h[L][this.EC](16),e.length==1&&(e="0"+e),Z+=e,r=45;else if(r==33)w=77,C=G,r=49;else if(r==88)r=L<h.length?78:73;else if(r==34)S=e9.next(),r=32;else if(r==47)G=C,C=a,r=30;else if(r==68)r=nD==x4?0:33;else if(r==45)L++,r=88;else if(r==37)Rc(249,104,A[2],this,A[1]),r=33;else if(r==97)L=0,Z="",r=11;else if(r==12)r=nD==Nl?98:18;else if(r==95)r=u?46:99;else if(r==27)B.length=0,r=33;else if(r==43)w=36,Tf(),r=34;else if(r==11)r=88;else if(r==77)r=nD==K8?94:68;else if(r==18)r=nD==Pw?69:91;else if(r==93)r=A[4]?70:65;else if(r==69)y={},q=0,r=15;else if(r==6)$t(WI(t.length,2).concat(t),4,this,263,166),r=23;else if(r==91)r=nD==iC?26:13;else if(r==98)d=A[1],r=19;else if(r==87)t=t.slice(0,1E6),$t([],4,this,263,197),$t([],4,this,263,36),r=6;else{if(r==29)return Jo;if(r==49)Ao!==undefined?(r=Ao,Ao=undefined):r=5;else if(r==66)r=t.length>1E6?87:6;else if(r==23)w=97,this.X=k,r=36;else if(r==44)r=nD==dR?37:77;else if(r==92)m=A[1][1],jC=WI((T=Q(this,263).length,2*(T|2)-~(T&2)+~(T|2)),2),k=this.X,this.X=this,r=10;else{if(r==72)return Jo;if(r==70){for(K in u=false,y)if(K!=="t"&&Object.hasOwn(y,K)){u=true;break}r=95}else if(r==10)w=91,z=U(this,260),z.length>0&&Yt(this,263,WI(z.length,2).concat(z),48),$t(WI(this.W+1>>1,1),4,this,263,87),Yt(this,263,WI(this[Zw].length,1)),oa=this.z0?Q(this,143):U(this,440),oa.length>0&&Yt(this,140,WI(oa.length,2).concat(oa),64),g=I(140,this),g.length>4&&Yt(this,263,WI(g.length,2).concat(g),65),W=0,W+=Q(this,181)&2047,t=U(this,177),W-=(l=I(263,this).length,2*(l|5)-~(l&5)+~(l|5)),t.length>4&&(W-=(t.length|0)+3),W>0&&Yt(this,263,WI(W,2).concat(dF(W)),53),r=16;else if(r==19)w=16,this.j7=yp(27,0,8,atob(d)),this.B=this.j7.length<<3,f(this,399,[0,0,0]),r=21;else if(r==83)w=97,r=34;else if(r==53)w=97,Bv(17,143,GU,this),Ao=5,r=33;else if(r==86)Tf=S.value,r=43;else if(r==60)throw A4;}}}}catch(ZE){if(A4=ZE,w==77)throw ZE;w==97?(Ao=60,r=33):w==91?(Ao=60,r=23):w==16?(GU=ZE,r=53):w==42?(Ao=60,r=4):w==36&&(Pv=ZE,r=83)}}}(),/./);(E.ha=function(a){return S9.call(this,2,a,35)},E.lS=function(a){return M.call(this,6,a)},E).AH=(E.vH=0,E.Vn=(E.KQ=function(){return pD.call(this,6,37)},function(){return CD.call(this,20,16,10)}),0);var yq,Hv=(gr.prototype[h4]=[0,0,1,1,0,1,1],Nl.pop).bind(gr.prototype[iC]);if((k4[gr.prototype.EC]=Hv,yq=Ww(27,9,gr.prototype.H,{get:Hv}),gr).prototype.a$=void 0,[])xD=J.knitsail||(J.knitsail={});xD.m>40||(xD.m=41,xD.ks=p8,xD.a=iZ),xD.uVa_=function(a,C,A,G,e,K,n,Z,V,k){return[function(y){return S9(2,false,9,y,k)},(k=new gr(K,Z,G,C,n,V,a,e),function(y){k.Vn(y)})]};}).call(this);'].join('\n')));}).call(this);</script><script nonce="5wz2kulV5Uv3sdz5Qfxitw">(function(){var r='1';var ce=30;var sctm=false;var sclm=false;var p='uVaShXLzLKiPKCihN+9K3D45Lm32/hzQ6iUk8KvWp6lvRfqojmdJmlMuhQ6/UHPyATp6hmGUq5Xhu7R+FS4XJPpiNtyoxxuEpjMjfn+ka4yKhQIg7Y9ZNdXNTgjPQL1QuiTwep3+ly9Q60A5jSs111H/zmefxFmVPA/av/hKDZx177mbgw9Wo+HJmnllQp215OijsArGPMhbe8aZbDyiPuT5+/UIpRllb98MXET32UahLhpAShdkaLo2j40AO5SKQ4wpe/o5yEqs5vXYciA7WT7u+GV+lnc+1kzArvTMj4+ucjzMlvh/XNDLXof0wKYrVcxDx8TpWpuw/vEpuTi77dYdgiKww/DG4qw/wPa2XrqGN3JVXG9LjcF8qB6tXc9mL4WCTpmc7+mQOdXSnHUtJ9DHSwkitzcdHNzjx5kX9E7Yjh/7Ac29dUeJGFpL4OGCRMqS/WOJ7AsN5k9Vs87Sv6ZM4ZiURQdMbSJYY0TVGPg9T7DovE7RhHCZjnBZI2//xtjzLzv0nceCcB/YvDNL8LJ5NKngXlIeoCtxotpRCEvR40WZ7w+rAuBmqS4yv8p/x0GePsyDq4nfxoIPAG90B4ktcCtUBWk5qvmR/nAudLVoAzI6VdRn/iWzlXe9HH3JvD8Qb/87EMg5jByPqF1ivrIPnEfhJB2FwzjBrabU1bVDzlC1GVmZDEEGfOis3J2EfkK3yaor3oZ1Z0F3si8JUDw0lgCJ9AmS0nJbA2wrh1kVwpvw1c1X3opozhZJPQ6h6bJVM5U0AicZ43VJlTct5wNuAMA80HB9taShYEoYKZd5o+U9mdI0Jcae+a/nmjfI1jiiJyKbS/nfokIqpUYM9eRecnngjUmCWab3YljD1gRQLKPG+9axLgAqmgLhta6w7hMtLrEwbvigI6dW4Y0aRl3JzTcq9TJnUk/oqBvo+T3rZ57UMRhlwjxrAEvoiS6s3YlGzvHIeuySqDOxRinMjH4wwum5AAyRoa+kYEKB2fU5SzGgBpI9F4FLlFv8zBqThP+SmJ5xmYw94mRK2/BcPCm2wJRBEeceMrL5YJqKhg9EkJiCSeYUvJlOdE6CNCz/MGj7Vl6Ime6N/Y+F4s8HrnbG7OkrdHW1zncrV/kwIyw2eFTwRfYwX07HHbRa513h3oyNsyrXlKTUW7mlEPVdfg717KWAX6wcKwEAriQM6LN9mfWH1miQwy2tmlYuYbEjFuVyEQKRng8WTa21xFLROaf4AGfodNvcVaGkn6cJnaxwTLR5PwWXYSMNHGPepCGx9BwjgMCwz0xmQQUbkN+4cfWGFYS/dzjDt1tm+tksEJrlXsGFaVGUQ0us+wRCI5avl7W9ms0XEb/EwKkCqAMQgGQXAwlKFj+ySodGt9yn6jkBljSgBIGO4mT923s+Nh0aWYHMdzTaVtf+5BJ2++ebHHDmpyNccT3unt8O77pxLUEG/7QVXIMhQpL21+X5g1GscfVwj64jcMiIGW9cAy3EwDWJVePKyFpF2f2MFLUlK8r8V7NoZjEQ0KaCaUn5PLvjV/vdiYnVHz9/sdlccZsy7zaM8Coc7ElIW93tNkkBJyQjiD5tyGtyCN5ONC82Xrg3ErM6LgNzSmlN+xrxiCJlEh4iTDej39CMEQH0TLwa/rlxksyrM1pITxhHGSLUxRLJhnsOEKTiTe9r7621IINUirnZal+Qg3Om1sczb7xJBpoBLPYuMlRL1rcRFtIeCDB/xAC++VrC1A6amYiz/j1WOJ14o9Wq0bc8o8o0E4G19oYQtb3tut1Ep46AUo/dqQlmMRTjGkslMwAgB76kEcxCEF5NRvlIRrYf1i+SL0N06OO54GjABdIK8WvsvWnvfJ+joVpuC0yNlx/PYv40Tuq5jHNzVk9Bqavc1jRzzDcM11uZR5ER0yST32fxaF28xRD4as3oiH39+HIJFWDtGuCLmR4P1N6Tp85vTx61XyvfyWb/W7tNt3yJYbOsc94v5yRHBAw8X1BYNwLCC2JZHUAx28lI4iuZybh0wJQnAutIGmPSh5QDH7lfHTNDgMPHFC8p5tT6ZZhklaBQ7V/DEw122JIHbYeNRdT4/kRrDkUMzd/ziQ2zSvTG0t/Hcz9qHAFeklUtc+m+uqgcNQDre2zdH6Jmv2FDp+9Ku448dtLBykahVajdzB/3JPMuizBJPZ5KO6tR0pqekHrG0peXGYxWqcQGcnu6h3fTWV8GyJq3Rzuj7Q+HFNUkQdBLqJuRglUTaHr1p7JiE75gS36OYxOP4t3LInOZ5dtm1kd0wxh+nLpKsmIG1sHFME6/H8rYtvpgk61lbydhx4IeJ176eOiA1RPvlqtFf2322xZwdtsXrfFTMiy+aT7PIBFUCzpIORNMlGCa2S12/GAjnbuASjYz6nouQ/4z+txuVQsOg3PABUtNI6hFDDk04K4EU6IIrENqItHpPWuBLRTgg6vgarbS0Td5pNgNcz4z+9BmZ3fm4ZEk00vvn5zXTfG6paE/MuQ6ykg0pRxvfi51w7mWAKHgY2DtmEVlk+ArCkw7GOEUJi/o6rghltb0CKDdfIeIFgxOIejuRo/DtUOXA7uzzUCfbIjnpF1I5bBxFbhoFv6bwNASn3HVY14q/9PFrI16pIbC3QIMy6l/2BO2RjUqAzj5pEB6PbUFq6ht5hXL9U3MXdMsPYXulRgNMBDkM2fgLWM5FGRwBiwcpOCD3/lLkNcbHsV7JQ39/NHozqD9qFndYe1s+hhkAkh5N9YZi8t/1FZQI5Cpcng5yYrASGEurF7C4rsYMj5azSAiR0cEMjtXuRJP2uoAp+HTRjenfx8297r4MMbjwsC1r2BDBQxXfr/S5SRpjURqqITxkXiHs17AM5sOXXY1zcp17EkQNEpx/SU6qs2YvdHWGvpQ9tnPnwRpb6zogl7ei9MdG17hMTrrM5Kwi9IW68gsLeltyJqCQF0IbWzSVDBhilN4fg5F6eojGOWpaxRqc+uJeJK/rqLGoAQwEnqwMwd2s0blyaD0V7l9swGwiyIajD9Ny+JKLSkJmtipH3D6RMF1P0JuWbc7IOaF7lOej/a+89GMpkAOmS4kvWBTxb7+vcWpzWnrl39Brb0xHbbfZatRfq5FCzlCP8jrOrGnc9aNusSdqjL+Be13LsHLwrNgHxLXBJIs0PJLrUtv0lRPmMp4hR4oQZKbs/46jOslkgtt3zmiGUfLIQCBSQgZELKyCcWIsAH6v/40QXvprN4AL77UTH1O64Xg0BEIRjbHX5FXB/19Gn5ww39WaFGyiXkoLqq5glncLoci4L7iVavz96B8xKhUnMIPxs+6wdPnC8v7NnHOQqNsb/Q+GK2ERyI2LBaOwV8XYPOOf4tkKzvVg+8gpExRrRLv0Cx/jMrkCB0SG/laSiE40YQopWoLv/421RgOfmxQHBEosxSgDtVIOEqjYqopqJ7H3EeMvGc7e2Y3++rmI1MnarnfJYZeRI+l/jheJq0sKcx2lMFV4kGISogSfFnpcYcvWwqLgN1LPx4ACyiw+BATTPkPR/gthQk2SqOZQg8UlrxzyMc3kXxTSZL3E+w4ySm9cNuypZGwlZrTQFAOcpteB2m6nU33/wjVNbariZn9BCpcuaQNajrs69s8D3ueO410uDYu8YFjHBZZ5Lnop/5qhO22n90VpjWm7zP/4THZaLkhqZ29qgbFZ240ivB6sf3u2hKcmypp4e/ojficmEWyrkKRRlg3/baMHMKgWUCRdKZkB9/PgenHfvaFgee7bIJQiKNvMslwZQyFUEb7w5aVf2Sx3YIEmw/tCLYWaJlqp0yZ8vK1cI3SI7asDbKJ1Bv45AiKBbIefm/Bha1gfmgCNXQppnRSrsiciCuntcd2o2yVmfXx/6WpIwewssIcniIjS1ZXmj2dLPcUkcAJzw3wk66g2ZpU1IbEuXixoaVstQlyWcOxbpnOmDsvPhEdpeLlGhRrrmN1+jwPyGRUpf5n3a03cIeQ8xtg08RHW2MsFeD2JIHfWm8aZmc/XwZjE6/08qRQGiYoxIdti7eh6J84uxs88NJH61eBTMxl5tfnl5EJYIcRvv/6/I76Y96mg9mvPXN7qJ3e7sBw+ZEX9+BaleOkW4t4qUj3T+TN3rhDmyaCKVoJuMhrex2epQH4+/AHhoBPoRzWUHp8BUOLxN8SwNiFzFvfRy780pYRxKXuMBo2SQnP3Bg2BzQHXcpV3sYIIMBTMSaaEQ27/QKNEM/mWZ24F1UKMbWkFTUGNhYXtLsmQitvylNjkF2SuOnZr4BsuftzOgAPLw8QZXNFre7j2va8oqKQBL1dDYxSm96KamgJ4+5ktwm10imRuQljixxjhSD0/sN0lDECaOtw7RB3pBeQ0KIPiEWzTHCxcftlgsIQbBUkUT1Ir2g86TZpRypCS0aJaGrE52HWXtFuMiRWvWgUd0DRu8DE3XkDez8mqjacowHErVCQgDfsQXvD1qjJAQznyFVJ4EBCAvLPcgWyLdpx0E69AHiUAOeaChGldEZmqll4Hwqs/HX1ccU9ZEyCII3houShT1RnssBs998bsXzpWLfmZG/O4p/XUn5glZhK6NaTsCx8Nf081njpBaiGXYkoc58FGPGfuhwckXMAVl1xX7sfuyURmZIMj2TD2byIb0ooH+/4g6Sjgsa1LkOb8SZVPA37UCyzjSuRxq017OseGG++uDFT0/C5+kZdHZdiUg2YHDX27cGvSY9BdiH7+6F50MZFO3EYsPtxj8RwLWrQnJB/ltVbnaDDJUlw2BqdO45Jw71hkJ2QwQSW7iJcYhvZHTCAnaaX+bxOcVrjsHIdoerIrxAsrn0mPEkABZOSf59Upv1UT0Qwxtjxpm3Oj/kounDbmqA2befw9t8UJxaztUIoWNi56gHOqRgWDuYanZ40Uk8NXu9fWKTzxpsKRrLl/NEIPaoncFI7HP10ByllWdydjjkq4q0019lE8qKsRdEvyQ8tWLJ6GIka5d6Wll+N7WSxrtWbX/om1YQEQeDJ2QGF+PuDYeUEosG7nDnE8R+/EiQ8wkXqpaiEHlk8Pu+5kLvF8s5TdXkJynaJ1VKY09HFQFl7SotcSOHDLKcdPdvq18BXJtoFBHnlr7QDKIVumFRPLQm2qxnEhbvlGBgHm9TyxI4MYpyzIu/ONnA/hEPiJ8fWmPFcSyQxcO586SpY1r6VJxMkHMsrtwFqfpO2hW0zgEE/h5pSRfGLAekFQfGTGzV6UqxRohQDkk+dOnslLM7tGXnsfZmpMUjhbbZMTDkIvb1Y0L6AfIEdEQPurydsEQcNdCS1pegPqBrV5MrCngogoo/TSTWCgrHGp71GrviNajBa9xw0nScBRGJnBEbNrLlrvhlUXpSee1PXUuoeQmykAUP7lsbtR5XD0SNJdVAlRI5a87sBvlxPyuBqzIUQoCXK0vRaiQlJWkoSqzxKK3xXCboTFYyPVMprULNcCMCNL//7F8V72H87AqUkfPEP4OcKcHfJ++KdwI0u8tiCM9OkFvk46L8b8dpAJMcldkGRnT7ky3cDaXDUsaOKj3Uo9B1StndgMYDQ+AREYN2VHpXPDv2PTqVmaOF1MPj4a6aXNnPg467S4G75EDIZKnfjd1cpx8XNCdJcoWmpk53ckRM4ImxGiroAkthzon2slVPB7k64zfjQtEEch4E58Lb3rgqU3FRitS+xlTYJnk3HgqmZven3bSsOAXYY1PPBFOOeq+dCmvYPLHQROniYKYjFz/+6brIQ5ScRQgwHCyV4wG/rvIKxOn8HLy1x/Yc3DTpm3RFIfl1OEsylqh7D2CjFT6CYJKT3KnZylamhjDKB9/afYMxMfoj5sk9YCMmOviDQaurJZQBMZl5pHdc9ROqahNdWgHPsZ7gTaErw1l5l4owQCkhhiSIZ3VepipzSI070hUY3fctH/Z7HC8SR2QeR9rIDDwauCjsUf3aPyuGcqhqLcVguEE4RFTkj6CF9Fj5lN6r2UX1778e3zsM158Vdu4txDw9+ikas5Gwl92Ey+vESgbTBLQ0qew2HGKvbs9sAvnBzSrUez25UJ+3l79JcRCnoEsUEO1iLHO2NGx2gxUBfi9WGaix9e2v7XLDe0/f4t0hOSTKGjIsVrqwhpZ23BZXQiGLTl4cotVMakK1JL0qwhgSbnl7EztCoT/lWQC+kWmqrDYV/VP+hjgu34/VBZbyIybsc6aIkMA9P2u7I4qp+PInpeRnZ1hQz1P3ZJ+wgMMk4nQxE89OG6r1VL4cdgvzqqh4sw0Dzp/ByqSTa7szs3vV6OGxQMe/1wL0b+hm3ZcTx3l1bppkQauuhGx4bcxALL3Sk3TDdq1f9Imqj0VWaGJEKtR4QfowdIikjPEnFh17qVUMrbZnPwo8K5fhxs8Agj8ppG+TwvxHht12gsqRx0uQhNAEqicaDR2EH6BGqh/sChx3qPkIgQjpRovRQ+53ZppIDxQf+bcF7OPw8+I/20davdIFgALyvZoPGv586h6EvGBv5q86Mes6S2p8rxNJ0KfTfe4ric73kh+/TnRLRFElI4aSLyJNFsgCBXS7QkkLRrfYpnp0YtaJZDj5e2srlfoXesjkrK3EgiM/5ebxJ3AWRB3khc4VB9/KOmXgMOURC6Cn9Sdmt7C+bmmFLJXMqor6YQj9TGHf1axYu/vVexnMitl1bGpUZwCMSzeHCNr4dauUAQkH/v5f7Af2poWGIRNH4vo/r4Wcilmg2V0fze3Lh5pmz/dHQkiMRRQNHzk7NdV+7M/6/g3zMaUy6KfyyfyzB41WarJTavp8QzRWIMKldndpld8rD1cjkw/L7SOrPUQru4z4qfmJrfSEvSEf02SFf26X73UWBMJaHoefU22D1+k2y4chVGtX0mkWwOiPjq/VR9DiaQwFCDDrrZyKRLviAC8yYtrVQoq5LqkrIxYFsMwTP3VUyfpugv5/Hh8h6TsbNVYAca8bwUWgFjys6d9oMj9ANtDIuYNi0f6jeieIeAJl2mnW452YZrjhdfsMnjWbUvov/kKXRIVH+/U5gySSTt1O2kq1ha4trjmCwOdsqMwHSfnwty2ULCZZlX0jhx50mqis8//9YUSC7fPpSHO2sn85URcGSU9jFwGwvpsifzGndYTFI3X8eav2c7W/Nz6v4ME+C1RAZp7VLBLbhyY4UbqGEvwUWElwn3CuBgYSVGwvMiNgzBp6rHzrF7NxvIsUsCd3xKSsNDQ2J22ouqOlWLSvfnHqjkOJcCw7Fra0smvqyoUyoHOcX3nfEseh98hdymk1OIusJBTbdbAfjdJiTWq4w8wdA+SzW4908YwjJRDr9X/k5WIrnfrdXIIsaaMq7c8E29fIwRpF1Jgk2U7SK7+ELFJHyMWUz70l0OUOUTQ2fw0UyL74sgHparVB+AUbbjFmMyYcavTY3CtekH8ffwKlN4rphYG16thFn+0iYg9UdBVYtXaUuRS2DZTbhTIpf/gn7GpnC8vzuwwcv4GUkg9JbI8zU1U8OswtRO86F/PopeLKxnLAddueNWfPz67AW9wchfYFSdOykELMg4rUaU7Lq0kGyjI2vcRPL88oC8Nus4CSz/kBdhWPXI76eZE55pGzYBactcLfQyQuH2h7m55uxcBDaPmRi3S2lioCsCGG+VrGkdTKCKYrx7kVzq/NiUnQCqSWwnlDWXVNYI7RgQDYuRqR6/gNEEoNdPDPH20v1HZL1sEHjQ92I8jXrEeyuMWGcZapQ3oUsk0rDcO2phckHzQlG+21HxVi1/0YmsBlZU8fBKARn6XwLNSkJs0ysAxw+l46fKZjsCUxUb0gFy70sgrPUiAKCaybC3DLu2Yr+DCZgjSTKFwtcTrtfuEjHXZ10bveYOzzC3u5EG/erjbU4vjjgkoIA8t4iC3qYXdyONgD2q/tgm4ajulpgPOhRmQaQTsRsjt+wpX7/dQ+Mopz+Eep9L9Q/ImoWXbLaDZU5ioICuYtOqrrgjxD98Gmz0fTQGkmrdOKdI50nFM2WtHZevztY9Pyu2nGjUzi07Yu+ZXE3Q6yWW27ziC0dfYRjnzRI9ibFKlEKJbG2o3sdojRZsLKhZczquLqYg5eifIr+IYBio9KPwsLF7cG0eMVKXEwPpPYcHjTt6tixc3llu3G66i3F0mlliII46e8b7b/0aidbZx8trCPIbWJKRW7kp0GwB2D7MDos/PPPefBcWWelZwG+6JrgP5og2DdEiIUe6NKko5AK/XYCAhleqVPZhWRhsiwes3T+tapA8MJZlsHL5jijct8hKv11cFvghtsjjdQr9pFMIn6XOoHPwJRBHFQOetcbOq+JADDn0uipnJ5JdB4zBxAssik7B6wAWcgrQMu72YIinU8dmXNPb0XoUHdVKtYDFY6BOZ4A7BlHlxYSzKuPxVWfS+Ue1abZfGoaqBKmoTTQFKQV5U9Se8iDFU1xIU4JSLjzy0nFaZuUIz7fqlUlf1wCk/yAqLG7NDeXEfq+iqvuHD0CoYkWX3eEQQSdRVtyAx7m81AwRVoMS7foBR+AhO5DjGHAPikvgoUmUaQq+35Bg5yhRs76FQ+MIHx//Lg0krfVAnjTEVguHrfPlN9SME2vyw0gYuvAXqXVMhCrrPOEgFjPRDbkqATUMwGdryGpWXTogyb6NK/Jfr3IJsJ/dq4jMaqH1iVjNN8uC9nPg2YSPuq/6o4dpQPEsXsqMPk/dhavB8wSANolaoE18sgWToK+HTazNEIG7emYJu++LjnvPHo7vvssTrIzXXT5lNguSxi6+xED6PKW7NrH0DtXjeRWXXTYmRqsexIXxl0J3p/cqbpDjJ0r9Y9F9opzCPt3S1U5sxSrqWITOdSIdXajUAZbijzLkU0cAGF8ugZerhxm9ppYRSfA9w8oJZs7AOLtUtYqsaGaa2swvAk4COXVTSeduudFwPoMN8fGiK0LPd3Itov2HETcgrYe5Edf/zW2vjabBj1HQJQQGy4o/cSM5JfONUaobAr2U7NUEbmEn+4HdeT0qCb4rHCnL9+Ow8PDk3q33omyOLdekjiq42ZmyqAJt693VEj6ZD6OOb9uahBg9PcjhX6ufgUf2BLNRZgq07I5hdh1QH1OVeVlg6R7k+LydkPSs1tMOjj1ZRlcRHZkbybS4EFxAgF+wdE+LGNIbyijnr7++AEiPdX7Y5W7MDSwyWNum5qK7u9lOzZqQBcJQtJjY0/FmVvZRD/lwWRTMAPNYIkso2hmUU8SyeONs4VHJTIIWkmT0pJjRNfS7+cGfui9jjhWWVUcLwgIIly5tmJEUiuCiEodaL2ed/024elye6kz03yJDBh0cEHceWhn5rSJcleLN92IIw7poKGclv0Grrc+c1fOhLOXCC01KBEAV0pwaR4ygA/hfPelOaeQ6OTlYivVQ6n61klYOg9Bo/JMsCbXP9o1AEjr8+8/OGI18tLDa6Oxz9/06zDcRfVFQpF85ZkWVrbWVSmH77TTq48wM9SY9BmUImK7I4XUw++6qrAY5ZL4bnhCFtRdkxlOoJQbrsokHeimFeBq7s6PPC2VtnCgEHvemyL7CCtkrDJo2+jnb8ebiAWw2AKfZSk2nU7BpI6ch7TI4my+eSjXHDAh4Gb7DKiHgTeL3ltjZe5N+jKVffb1q5lZObMSrES+tgopM8rjfVyduNV5F100U2SZms6MmCH2CpjISVWJoSklFNsWHht7Pck1vNgnPk+BRbygFlZpdnWCCliEA9knTMTLoCntnydqDF/mUVE1z7lbGqat4vhy4Ry+472WaGqCBV+fXxuhy1kxb8xxZ6h8XcSFy/DRI4/UPROCURtQhIgjN3oiFJ9sj/EZx65thMNFsoVvwLdzQf0VlGX1OpO30A3uZMTvVd2A3PvAlhI+4ygAfOH+B3eK0z/IStaWCsM6mEOv59xPtAgGYZTNNgKmkYyNvCYYCe0f8m+m0yStLjjbr+MIW+7B43v8AglroaGXVqKMQXk+BHytICnl4P+dSXDrtMkZpTqrQcz5FtDPW7kFjiJqu8RsbwuvkqUE28i41LTj/v3z9mJdm2RbLGwETjRzAoz52sKhF3NF132JVuWLOUc4tG+gClVwnPmLhiiRNxZJxkhWXISFEEP4N5GGy459kpwwGTl4IQVLBscPYc2UACWmn23q+4Fc7HnOp+5oOFaJJNdAZ6UFs59B7TDSqoHuYhXB5xJjGhk9yFIznuVN5jJCJ1FOqiXL3cgjJAb5wLx625sCI6vUAwS1p4jjJ5xlMeXBcYOBcbUWbSjuKzt8RtGMkBo2WertnUo+Vh2D8fdaVzKB4QvvmOfTV4jH9FmIyU85NxTL0wnxhn61nET3UchdVAEZP/+yqOuJriCml7iiAmyvsTabscC4jlf0GWfx0hc8uf5dyUSk+X/gBC8zn9x1vfS3UkARAl79kO6N3DJSLqrGBZxXSDr3mpUGb2rJ+uhNlbtElLv8Z/gBdwtRuIYkg0Nyaq88XcEI1KenfqiCx5m15O5tDLH4nA88WloN6g5hvP/oNWMWPc5868Z4dc6ieC8rVFVchD5VC2UZVuZ/ZM98fIuY7ShsIagUDO94WX0vfDr5tZqz5vtp7t+JFXney2/bpVymj9S2mjWgO5N4D8vPhX16rQCfo/qIeGUqEgnMhesCtPTlOO17F0A17x3htorvPYM9hbIXzu15k6w6SPlkTbnrhSP8rjhdyXEvwMGfgfCD/j/GGGuwCAhDqIIyLBTZZCDuJnBK2UFoKxk7nYW9as0Sv04AOR7Eo3upvCD+Ac72f5bu7CtBKEieIGK+TcsbELC874QDgPfPxSCiim1m2AyYQ3dnINg09DdJYS5nlIjPo273ao0w/pZMBq99vZJkLiF1P/BdqsLIiYPAbWkZl8qrinUqSifw4C5ewyzz8GbrgA2SxPBslMaJR+AQjC8lLqYOc00a/l7QOeIHAT9eh9SVrZ7m0nNPuxA7iYgB1NxbP6JMDG7ajEXKsMs1Ig0IsQQWZ5QUQxcg5YCs3JGoRfByUDhOZMw6cPyFvSDAev2uX3Er0JQGj+gg1r0BfMtfoolnisQ0GESai/YmfAdelDr2JHdsd8a1fsHZTsyCvN7UcRDKR3EhwxcqpMEAPYwRoozuD8vkvwA4iAAn53qMIgYz/34QUnAB6ko5gAHVbYiwImPYSDVxJ6L1PcY9e4eyy8vUWjHnqdyQcHzHbC5Ku0WjKs5U6vUvejSs40x2P+ZZG30MU2v7VT8PxfTyx3j208pFmKQZC2TYlqJ4oZzp211aJb5LnJeZfehb77+PIs2PoaeqVq+YlbFFGLxNqh47YoxazLRdqKjc2jEGzp4SBOhvb8S/cNrUU/UhG/njoeiuNyaJDzmPSjXZHWSeezmMdwjQqVBDz3JPQ8B6V7baoBFu3IMuJB5DYlKQ/tsCf3PM0zY+47e726GKQIp+umssJ9a9A91mYlir4zzTIBXiLcPcBu98PHz/EiBncKd1CGwCjOuAIW30AerhZ7Y46MBy9VQungO3Dy7r5jpScZnasGBs1uU6Rgb6iPRBUuJo5LgIERmVG/G77niNWCoZvuuG0bNP/D1uXzZloVlecKe3z9ji2yqT6ZeFU7guJOR6VefdNgswDA2Cn9QSUlgDdK8SWa5kjftadpVxEUbEFIoo47u3Ppkh3kMEV4nEUcTaLNhXzgaoyVsZXc+2qSD50G18M0I8LTGsbaHwV39YL/h36JHbZyBtkyd0svlTcu2OxoefR2q8ZCmtrdtey9AL5iG8znI6eLW5RjJKe/tQxttiPv1QPwEKEAzqUqju2LSLVcaaZFS4NdVLahRpc1vYCxfwaA4lLqtwI6jXOfpJ1MiodTLafX/RjlaQ9U/uiebeOUujUFaFhEQCtVy17Rj53zf6D9jQ4OhdB2VAN6151C7SpThYkr9p75wQgS+23xtikByuAdDzgex8fS3Cj8rBvw5N9RQouWTf4Uv5AGJNlxlebBBL+bPDJmYuccyzKilipOC4ow/bV+Pth9BEsUvWYfxMpa0lRI/aCgDmndkiI0Sz7uOpczDM1dW97fwjqI/yusjLlQz1EY5tayfEu4Br60wmTk7dfTQxF7MvUkvhZXcHRWqvJ3WI9xcN6BG5FAOcwFYsScScEsLNFTWrM3MeWho/iKU438u5Mhj4uXbZVh+I/9QQSLTG9NEcK+So7+9oIr5tZEl6wlEBVNMlm6CHB+LCnuLCw+Lva2c3juRGkq2wcMS4cZcizKlgR5y4oBGGhHIKJG5vXD+krlcD6zLFSrxOpKjePL5GnSrr+iwJ76c6Cxt3AVCrgaf8cvRbsJ8R/bdx6kvMH08FTjBOg04IGPIf8IUe45PP06KLloCnIbea/bh0OL5gAVRQxUZ65uql5osy5dYSdpISyk+Wr50vxLvdTtqCG4E4nx2DHIb69xco6ImqKKlSeI0l1nikSmqVEUPszDQ3rSP/+qRjxrQz2cvKXHPob8aFfHll3tuB++o7vTU76L+ucdqgQD/UQxG56pnZt+bgPqnuQUZkG5G2xtyFGzAfJZr6aaFTeSIKJ86XXocnYm5fyLK+Ehksh8C09ywmM9f/KCORshSXYJrH0NELVrnZdAG9bvkuBzVCbZPf5xzVcCEeW5DOkVDyNKRj3gpdyw21Npt4w+z5di8gO4qtFFshtPhRVqFMmys+ww4a6ujSZsOPUxyz67hb0eUnbk4gkzb65Lao2Ifxy9+5S/xzpgUfF3/K8SRXl8Yt3Fp7HnN/2i6lC3hymNr4RBPKiLhGP0n6r6fBzxDQ7LK1spVlkxksv3PDyv3W64Ep+J1YkRz7U3ufvtJRPs3LF4Eu7wfplgM0lcLUSSjgITiFcsFNG4lx+Yw0+ypCa71r8mSyhdJvhvQbk9OgZ3Z/Dd0AJ85LFBgdlY8ygiY4rEvlUZLumdACbPuGk2miho0mWlwfM19qXTuK4xd6QwJquLaCejxwyK/egJ5OVSzbsUpvOfANU7s0jc0wnPsmVowonAQ/CNip692bdNghT4Gy8Y50GEM71iq3tmtExqyWUasrGQlPpsAjuwOifkd/XEOg2n6Y8jMFqDR2kOFHSLt8l/DU3Oey2RCcPHPhP7p2bnedUizKSCfOObdSSutfmbhPdVEJZNrNSfKocgyl4oqTgyDvgfohJNvT5n7ZY3372PpgwSrbSzEFl03AVHH2smsU6BUDWS+fb7ZKNoISWhzgbM1JMZT+VHqcTCrihPijelzMG/JxHmpajb4TbfL8z/o8ci2+BfbLSUGiM2wnJKeskaIb9qWVK3kRM+L7EMT5fmet46sPBD+sKIOQv7K3yJPztn/bYF5ZtCAy9hoHv3h3VLWd8cv5AIxFxxJ+S2FJ6AnxsmIGfA1Tx3L2PY/CD8aZv+CEM0nHh3fl2qdUIDCjBB9OHKhxjzuozMCnpmvsBOjHd9vh7k/U2vBdmyWGGRLOUeihHOON/J2A+2M0/xEb23yo2Nv2QdEin0EBeJwCh0HJWc3xuLU8c4ZOSSM+EzXPV13H3khxMZsNQsEtYhm+bAej3OOjQg+nwwEY1sIE+KLbWP68l4m57FjIZiLStPbcM60jYIqPx2Cre5Y17434b3C3E7PNDYvOdadPA5eKElI+00AMrPEEbrEKerHczCzcHnxGHZyVhCEvx2n5lFEAV908kEZ3YsXRdwi2Pg+xV4MAfP7TNnF+s0TUMe5kiXVkNjUWxF6+kKFxaVukhvPiTbB2ZRfIVNUIXPcUTV28L0w8vApOwbDjKdnsSqbM/dyD9pNzsGF9jZCyOJojtK48SuxQgihNtX3E903S4lHGppZyJXjSuRwfNGxTFYP+ukMGfBXFQv59YT6AipN9L48c25hVmlBMCtoRGZGrW84l814CoYc7XIp0VNkaCQcfwuMr04x5/vPyC+zRTyPY8s2RgboXqiR5DFs3wH5DJh2WiTSgSuv/kmLeliaBbENpmUXTR1LlrJYlHxy1dDHJ5jBpnt9KS5v95RrISeC7eIuPQBLWXkFdiIuDhOkNkLmnjtN4gzRRsAYyqnOgjXaf4FP+/pXRfgteoiL7x+5UYSQL1KMEYIHFDYJDJ9BcIkrszmPTiq78CwnEM288t/mrDHySGbkkaC1+96eFCN/dTkYKgpizecIT+vFLFzEtAq2kVkt9Ff+hbTW536Vj4Vij7rZyjqJ1QRZ4m6Qqlyi65Dmdf73GKtYgYT8mVv1hz/61D2P0Js920MQGJypndbcMNCHbzQzYbaXZ4oU3GzAXRHOVoqMr6Yii69PFqF0Fv/fCuxy7MR9zvYoUXfdtb992jSiNKCtGZWVQkWwpQ/Hq2Ec2n/UL9scOS28f9WDMEveGPfn3om4VCMfAoIP/EOZs/rIhO/J7JHCl09RThWbzwOooHeF9UB4PYS9rxxOzmEYilozniHrVg3+LYl08b4lYc41qwRn9iuOGG0A8U/QCHO8zTssqADrE8nTDR6HvfpK/HZKaL/+v55/pbST2jgLOs6ZxQHewDbewlkBFqWTK/GtjHp5560HzfOvpyMHjJGFPygj1Gl6aH4bYVY0d7c9X6qg9q0fKZKlfhQnmyry+oyEI3gRnnZLkNc4g7JM5wLNiwKOYR7bwLqmHIBjjqbnM2cq4VHTRZa7ttelHGy0RAdnksg7Bn5Y0GoXScox75V/KiXefmCDpadloXo8F5N1sKRmL7FeEkaUqWy8/fB89eOv8pBzrBz9ZJp6zpH1wJS7J2fw7nzjTLOS4slhdpggfeO4yqWGyTy6ytfYRFJfwUOG5J+VYj8zm72KSPEV2f9tB0Y2Zv2kmhVdV4jGnI4iuCGSe0ewrCgpN6cNviR9Mlb3GDjqTC1lcJs+reDGLuJQFqCPHh4z4wVU46D5URFzb2fXQVkCVnJMuVVblYSHBBPUTYCdO929AhWG5OydL9hdmZwqpvcqHKvbGjClDM6YYEMEKOnNwNdajyWBerhPN9D7XcwreqHdN0bIDd1D7Fw4uYqnJYhS7gJoY6mTe1MDnlyCAU3omGE99dBQlR8UKFcetQXclJTH1Aw93KZa5QrXsJy3xApl2NKUhIPkqV1TwDvTk4kKZxNJhBlKbUo3fE9BS7nDTIwoJ85M8KSrAQUyrux2wvKyqix8WILWLqWSXuKMI4faMLEDKhArHN8ecETUB6g6kyX2hCpzthDPqH6TH8Ln+fiVw2r0OmKcLrOWjOxqdCKACBXXwjZXHIml6BCvwVyZmoSbQOfUDhH2mn7o+deKiFg5MGTb7jrS5YkyH4sCr1z8QE33osbLOGE4qlB1vIwcaY45jvo4TWOliCbJ9Z6/LSqCv62ESS7/LTTNOwcHBGWTJpaoLvxbD/vQXRHofpaJI7fQ+ndV9NYrfKwe+TrA+c5sD6eyEajAGR4yB8YbZ4zrMI6Ir0T5Zy1HKAx6xEJxHLpj1F8e5NDRyuX7HRi+l0JCCCQSJpjNZrMU3N3XEghUD5Y9OV5sYtF9gxnBo/8zEJ88uJMNd9Bzw1N63jjLVUAJbIWQPoBBaR/tmf597b2whan6gaeO9dO8C+vhXmR+96BuBIeXr2pBZm6MmFxLkX4Zn7DRqIybcSLDuMN4UU4g+hLamptdldvcEkLZB3pY9SkUJg0RON8r8D0FAvFCy+YdTUDrqMsburzFVdyVrwg9//ZOAsPZPxunlkTJXjfpdgk6VewOO3MX5tbaEFbFn+VL2mRnNIMfMM3Gwzw7Hx0bCC1kxY0wHBf4d0Q9/jeC4OFAxQWjgGVunYZdw3KP7DOW2re2PK5vLkhpNCZwsRMDEwRdo2dw/G/q4oWvpZw+kzbmEbWJ55W7rIcWb97j0FLAtFhjubgH6nDIuCnUtSOk8StWtqFyRZAcvijlYel0RWi/yXam19I6oSAjQQ6Ty8k6mokd8u3wKVF9KB9g';var g='knitsail';var eid='KCgdauDQAambnesPlZDHmAg';var ss_cgi=false;var sp='';var hashed_query='';var cbs='';var ussv='';var content_bindings=[];var challenge_version=0;(function(){var t=typeof Object.defineProperties=="function"?Object.defineProperty:function(a,b,c){if(a==Array.prototype||a==Object.prototype)return a;a[b]=c.value;return a},u=function(a){a=["object"==typeof globalThis&&globalThis,a,"object"==typeof window&&window,"object"==typeof self&&self,"object"==typeof global&&global];for(var b=0;b<a.length;++b){var c=a[b];if(c&&c.Math==Math)return c}throw Error("a");},w=u(this),x=function(a,b){if(b)a:{var c=w;a=a.split(".");for(var h=0;h<a.length-1;h++){var f=a[h];if(!(f in
c))break a;c=c[f]}a=a[a.length-1];h=c[a];b=b(h);b!=h&&b!=null&&t(c,a,{configurable:!0,writable:!0,value:b})}},y=function(a){var b=0;return function(){return b<a.length?{done:!1,value:a[b++]}:{done:!0}}},z=function(a){var b=typeof Symbol!="undefined"&&Symbol.iterator&&a[Symbol.iterator];if(b)return b.call(a);if(typeof a.length=="number")return{next:y(a)};throw Error("b`"+String(a));};x("Symbol",function(a){if(a)return a;var b=function(l,d){this.i=l;t(this,"description",{configurable:!0,writable:!0,value:d})};b.prototype.toString=function(){return this.i};var c="jscomp_symbol_"+(Math.random()*1E9>>>0)+"_",h=0,f=function(l){if(this instanceof f)throw new TypeError("Symbol is not a constructor");return new b(c+(l||"")+"_"+h++,l)};return f});x("Symbol.iterator",function(a){if(a)return a;a=Symbol("Symbol.iterator");t(Array.prototype,a,{configurable:!0,writable:!0,value:function(){return A(y(this))}});return a});var A=function(a){a={next:a};a[Symbol.iterator]=function(){return this};return a};x("Promise",function(a){function b(){this.i=null}function c(d){return d instanceof f?d:new f(function(e){e(d)})}if(a)return a;b.prototype.j=function(d){if(this.i==null){this.i=[];var e=this;this.l(function(){e.v()})}this.i.push(d)};var h=w.setTimeout;b.prototype.l=function(d){h(d,0)};b.prototype.v=function(){for(;this.i&&this.i.length;){var d=this.i;this.i=[];for(var e=0;e<d.length;++e){var k=d[e];d[e]=null;try{k()}catch(m){this.A(m)}}}this.i=null};b.prototype.A=function(d){this.l(function(){throw d;})};var f=function(d){this.j=0;this.l=void 0;this.i=[];this.D=!1;var e=this.A();try{d(e.resolve,e.reject)}catch(k){e.reject(k)}};f.prototype.A=function(){function d(m){return function(n){k||(k=!0,m.call(e,n))}}var e=this,k=!1;return{resolve:d(this.J),reject:d(this.v)}};f.prototype.J=function(d){if(d===this)this.v(new TypeError("A Promise cannot resolve to itself"));else if(d instanceof f)this.L(d);else{a:switch(typeof d){case "object":var e=d!=null;break a;case "function":e=!0;break a;default:e=!1}e?this.I(d):this.C(d)}};f.prototype.I=function(d){var e=void 0;try{e=d.then}catch(k){this.v(k);return}typeof e=="function"?this.M(e,d):this.C(d)};f.prototype.v=function(d){this.F(2,d)};f.prototype.C=function(d){this.F(1,d)};f.prototype.F=function(d,e){if(this.j!=0)throw Error("c`"+d+"`"+e+"`"+this.j);this.j=d;this.l=e;this.j===2&&this.K();this.G()};f.prototype.K=function(){var d=this;h(function(){if(d.H()){var e=w.console;typeof e!=="undefined"&&e.error(d.l)}},1)};f.prototype.H=function(){if(this.D)return!1;var d=w.CustomEvent,e=w.Event,k=w.dispatchEvent;if(typeof k==="undefined")return!0;typeof d==="function"?d=new d("unhandledrejection",{cancelable:!0}):typeof e==="function"?d=new e("unhandledrejection",{cancelable:!0}):(d=w.document.createEvent("CustomEvent"),d.initCustomEvent("unhandledrejection",!1,!0,d));d.promise=this;d.reason=this.l;return k(d)};f.prototype.G=function(){if(this.i!=null){for(var d=0;d<this.i.length;++d)l.j(this.i[d]);this.i=null}};var l=new b;f.prototype.L=function(d){var e=this.A();d.B(e.resolve,e.reject)};f.prototype.M=function(d,e){var k=this.A();try{d.call(e,k.resolve,k.reject)}catch(m){k.reject(m)}};f.prototype.then=function(d,e){function k(q,v){return typeof q=="function"?function(C){try{m(q(C))}catch(D){n(D)}}:v}var m,n,E=new f(function(q,v){m=q;n=v});this.B(k(d,m),k(e,n));return E};f.prototype.catch=function(d){return this.then(void 0,d)};f.prototype.B=function(d,e){function k(){switch(m.j){case 1:d(m.l);break;case 2:e(m.l);break;default:throw Error("d`"+m.j);}}var m=
this;this.i==null?l.j(k):this.i.push(k);this.D=!0};f.resolve=c;f.reject=function(d){return new f(function(e,k){k(d)})};f.race=function(d){return new f(function(e,k){for(var m=z(d),n=m.next();!n.done;n=m.next())c(n.value).B(e,k)})};f.all=function(d){var e=z(d),k=e.next();return k.done?c([]):new f(function(m,n){function E(C){return function(D){q[C]=D;v--;v==0&&m(q)}}var q=[],v=0;do q.push(void 0),v++,c(k.value).B(E(q.length-1),n),k=e.next();while(!k.done)})};return f});var aa=function(a,b){a instanceof String&&(a+="");var c=0,h=!1,f={next:function(){if(!h&&c<a.length){var l=c++;return{value:b(l,a[l]),done:!1}}h=!0;return{done:!0,value:void 0}}};f[Symbol.iterator]=function(){return f};return f};x("Array.prototype.keys",function(a){return a?a:function(){return aa(this,function(b){return b})}});
var B=this||self;function F(){var a,b;return(a=window.performance)==null?void 0:(b=a.getEntriesByType)==null?void 0:b.call(a,"navigation")[0]};function G(){var a,b;return(a=window.performance)==null?void 0:(b=a.navigation)==null?void 0:b.type};function H(a,b){google.c.e("load",a,String(b))};var ba=window.location;function I(a){return(a=ba.search.match(new RegExp("[?&]"+a+"=(\\d+)\\b")))?Number(a[1]):-1}
function J(){var a=google.timers.load,b=a.e,c=google.stvsc;c&&(b.ssr=1);c?c=c.isBF:c=(c=F())?c.type==="back_forward"?c.notRestoredReasons===void 0?!0:!!c.notRestoredReasons:!1:G()===2;c&&(b.bb=1);((c=F())?c.type==="reload":G()===1)&&(b.r=1);if(c=F()){var h=c.type;h&&(b.nt=h);h=c.deliveryType;h!=null&&(b.dt=h);h=c.transferSize;h!=null&&(b.ts=h);c=c.nextHopProtocol;c!=null&&(b.nhp=c)}(c=window.navigation)&&(c=c.activation)&&(c=c.navigationType)&&(b.ant=c);b=a.m;if(!b||!b.prs){c=window._csc==="agsa"&&
window._cshid;h=!!google.stvsc;var f=a.qtt===2;b=I("qsubts");var l;(l=F())?(l=l.type,l=!l||l==="navigate"):l=!G();h=!l||f||h||c?0:a.qsubts||b;f="r";h>0&&(c=a.fbts||I("fbts"),c>0&&(a.t.start=Math.max(h,c),f=a.fbts===c?"i":"u"));l=a.t;var d=l.start;c={};a.wsrt!==void 0&&(c.wsrt=a.wsrt);if(d)for(var e in l)if(e!=="start"&&e!=="sgls"){var k=l[e];c[e]=e==="sgl"||e==="sgsrt"?k:e==="prs"?d-k:Math.max(k-d,0)}h>0&&(c.gsasrt=a.t.start-h,e=I("qsd"),e>0&&H("qsd",e),H("ests",(a.qsubts===h?"i":"u")+f),a.qsubts&&
b>0&&a.qsubts!==b&&H("qd",a.qsubts-b));e=a.t.sgls;e>=0&&(f=b=c.sgl,e===0?f=0:h>e&&(f=h-e),b!==void 0&&(c.sglo=b),f!==void 0&&(c.sgl=f));a=a.e;e="/gen_204?s="+google.sn+"&t=sg&atyp=csi&ei="+google.kEI+"&rt=";b="";for(n in c)e+=""+b+n+"."+c[n],b=",";for(var m in a)e+="&"+m+"="+a[m];var n=e;m="";B._cshid&&(m+="&cshid="+B._cshid);(a=window.google&&window.google.kOPI||null)&&(m+="&opi="+a);(a=(new URLSearchParams(window.location.search)).get("gemcid"))&&(m+="&gemcid="+a);(a=(new URLSearchParams(window.location.search)).get("gemrid"))&&
(m+="&gemrid="+a);n+=m;typeof navigator.sendBeacon==="function"?navigator.sendBeacon(n,""):google.log("","",n)}};var K=function(){var a=location.href;this.i=this.j="";var b=a.indexOf("#");b>0&&(this.j=a.substring(b),a=a.substring(0,b));b=a.indexOf("?");b>0&&(this.i="&"+a.substring(b+1),a=a.substring(0,b));this.l=a},M=function(a,b,c){L(a,b);a.i=a.i+"&"+b+"="+c},L=function(a,b){a.i=a.i.replace(new RegExp("&"+b+"=([^&]+)","g"),"")};K.prototype.toString=function(){return""+this.l+(this.i?"?"+this.i.substring(1):"")+this.j};function N(){var a=eid,b;a:{if(window.st&&(b=window.st(location.href)))break a;b=performance&&performance.timing&&performance.timing.navigationStart?performance.timing.navigationStart:void 0}if(b)try{window.sessionStorage&&window.sessionStorage.setItem(a,String(b))}catch(c){}}function O(){for(var a=eid,b=ca(),c=[],h=Object.keys(b),f=0,l;l=h[f++];)c.push(l+":"+b[l]);if(b=c.join(";"))try{window.sessionStorage.setItem(a,b)}catch(d){}};
var P=function(a){this.i=a};P.prototype.toString=function(){return this.i};var Q=function(a){this.N=a};function R(a){return new Q(function(b){return b.substr(0,a.length+1).toLowerCase()===a+":"})}var da=[R("data"),R("http"),R("https"),R("mailto"),R("ftp"),new Q(function(a){return/^[^:]*([/?#]|$)/.test(a)})],ea=/^\s*(?!javascript:)(?:[\w+.-]+:|[^:/?#]*(?:[/?#]|$))/i;function fa(a,b){var c=B[g];if(c){b=c.a;var h=[ha()];b(p,function(f){return void f(a,h)},!1,void 0,void 0,void 0,void 0,!0)}else b(Error("f"))}function ha(){var a={};content_bindings.forEach(function(b){a[b.key]=b.value});return a}
function ca(){var a={};if(!window.td)return a;var b="n",c=window.td.ns,h=void 0,f=void 0;window.st&&(f=window.st(location.href))&&(c=f,b="u");window.td.qs&&(c=h=window.td.qs,b="i");if(!c)return a;a.st=c;var l="r",d=window.td.rs;(h||f)&&window.td.fs&&(d=window.td.fs,l="i");d&&(a.srt=Math.max(d-c,0),a.sts=b+l);h&&f&&(a.qd=h-f);return a}function S(){var a=eid,b=new K;L(b,"sg_ss");M(b,"sei",a);return b.toString()}
function T(a){var b=eid,c=new K;M(c,"sg_ss",encodeURIComponent(a));M(c,"sei",b);U(c.toString())}function ia(a){if(window.prs){V("psrt");sctm&&J();var b=S();window.prs(b,a).catch(function(){T(a)})}else T(a)}function U(a){V("psrt");sctm&&J();window.prs?window.prs(a).catch(function(){W(a)}):W(a)}
function W(a){if(window.pr)window.pr(a);else{a:{var b=b===void 0?da:b;if(a instanceof P)b=a;else{for(var c=0;c<b.length;++c){var h=b[c];if(h instanceof Q&&h.N(a)){b=new P(a);break a}}b=void 0}}a=location;if(b instanceof P)if(b instanceof P)b=b.i;else throw Error("e");else b=ea.test(b)?b:void 0;b!==void 0&&a.replace(b)}}function X(a){navigator.sendBeacon("/gen_204?cad=sg_b_e&e="+a,"")}
function ja(){V("bsst");fa(function(a){V("bset");var b=challenge_version,c=cbs;a=b>0?"B."+b+"."+c+"."+a:a;b=new Date;b.setSeconds(b.getSeconds()+(Number(ce)||300));c="SG_SS="+a;var h=document.cookie.length+c.length;r&&(h<4093&&!ss_cgi&&(document.cookie=c+("; expires="+b.toUTCString())),window.td?O():N(),ss_cgi||document.cookie.indexOf("SG_SS=")<0?T(a):U(S()))},function(a){X(a);V("bset")})}function V(a){sctm&&google.tick("load",a)};navigator||(B.navigator={});typeof navigator.sendBeacon!=="function"&&(navigator.sendBeacon=function(a){(new Image).src=a});window.onerror=function(a,b,c,h,f){navigator.sendBeacon("/gen_204?emsg="+(f instanceof Error?f.message:a)+"&srcpg=sgs&jsr=1&jsel=3")};if(sclm){var Y;window.td=(Y=window.td)!=null?Y:{};performance&&performance.timing&&(window.td.ns=performance.timing.navigationStart,window.td.rs=performance.timing.responseStart)}V("sst");var Z;window.sgs&&ussv&&sp?(V("ssst"),Z=window.sgs(sp).then(function(a){V("sset");r&&(window.td?O():N(),ia(a));return!0},function(){return!1})):Z=Promise.resolve(!1);Z.then(function(a){a||ja()}).catch(function(a){X(a)});}).call(this);})();</script><script nonce="5wz2kulV5Uv3sdz5Qfxitw">(function(){var cssId='yvlrue';var eid='KCgdauDQAambnesPlZDHmAg';(function(){setTimeout(function(){document.getElementById(cssId).setAttribute("style","");navigator.sendBeacon("/gen_204?cad=sg_trbl&ei="+eid,"")},2E3);}).call(this);})();</script><style>div{font-family:sans-serif;color:#545454;background-color:#fff}a{color:#1558d6;font-size:inherit;text-decoration:none}a:visited{color:#681da8}</style><div id="yvlrue" style="display:none">If you're having trouble accessing Google Search, please&nbsp;<a href="/search?q=finite+resolution+trivial+module+induced+from+Sylow+p-subgroup&amp;sca_esv=6f5a7f2fb1e9ae4b&amp;emsg=SG_REL&amp;sei=KCgdauDQAambnesPlZDHmAg">click here</a>, or send&nbsp;<a href="https://support.google.com/websearch">feedback</a>.</div></body></html>
codex
Google search is blocked with CAPTCHA. Let me try a different search approach via the arXiv API more carefully.
exec
/bin/zsh -lc "cd /Users/mac/Documents/Rethlas/agents/generation && python3 -c \"
import urllib.request, xml.etree.ElementTree as ET

# Try the arXiv API directly with a very broad query about modular representation theory
query = 'all:Green+AND+all:correspondence+AND+all:modular+AND+all:relative+AND+all:projective'
url = f'http://export.arxiv.org/api/query?search_query={urllib.request.quote(query)}&max_results=5'
try:
    req = urllib.request.Request(url)
    resp = urllib.request.urlopen(req, timeout=10)
    data = resp.read().decode()
    print(data[:2000])
except Exception as e:
    print(f'Error: {e}')
\" 2>&1" in /Users/mac/Documents/Rethlas/agents/generation
 succeeded in 11016ms:
Error: The read operation timed out

codex
arXiv API keeps timing out. Let me shift to pure mathematical reasoning and construct the proof from first principles. I know enough representation theory to do this.

Let me think deeply about what the correct proof approach is.

**Rethinking from scratch:**

The problem: $G$ finite group, $P \in \text{Syl}_p(G)$, $k = \overline{\mathbb{F}}_p$. Show $\mathbf{1}_k$ has a finite resolution by modules $\operatorname{Ind}_P^G(V)$.

The key mathematical structure that gives a finite resolution is the **radical series of the group algebra $kP$ of the $p$-group $P$**.

Since $P$ is a $p$-group and $\operatorname{char}(k) = p$, the group algebra $kP$ is a local ring with unique maximal ideal $J = \operatorname{rad}(kP)$, which equals the augmentation ideal $\Delta_P = \ker(kP \to k)$. The algebra $kP$ is Artinian and $J$ is nilpotent: $J^r = 0$ for some $r$.

Now, consider the short exact sequence of $kP$-modules:
$$0 \to J \to kP \to k \to 0$$

Apply the exact functor $\operatorname{Ind}_P^G = kG \otimes_{kP} -$:
$$0 \to \operatorname{Ind}_P^G(J) \to \operatorname{Ind}_P^G(kP) \to \operatorname{Ind}_P^G(k) \to 0$$

Note $\operatorname{Ind}_P^G(kP) = kG \otimes_{kP} kP = kG$.

Now, $\operatorname{Ind}_P^G(k) = k[G/P]$ has the trivial module as a direct summand. The map $\operatorname{Ind}_P^G(kP) = kG \to \operatorname{Ind}_P^G(k) = k[G/P]$ sends $g$ to $gP$. The augmentation $\epsilon: k[G/P] \to k$ sends all cosets to 1. Compose: $kG \to k[G/P] \to k$ gives the ordinary augmentation $kG \to k$.

Now, the radical filtration of $kP$ as a left $kP$-module:
$$kP = J^0 \supset J^1 \supset J^2 \supset \cdots \supset J^r = 0$$

Each subquotient $J^i/J^{i+1}$ is a semisimple $kP$-module. Since $kP$ is a local algebra whose only simple module is $k$, each $J^i/J^{i+1} \cong k^{\oplus d_i}$ for some $d_i \geq 0$.

Now, the key idea: **build a resolution from these subquotients by splicing.**

Consider the short exact sequences (for $i = 0, 1, \ldots, r-1$):
$$0 \to J^{i+1} \to J^i \to k^{\oplus d_i} \to 0$$

Apply $\operatorname{Ind}_P^G$:
$$0 \to \operatorname{Ind}_P^G(J^{i+1}) \to \operatorname{Ind}_P^G(J^i) \to \operatorname{Ind}_P^G(k)^{\oplus d_i} \to 0$$

All terms are induced from $P$! And $J^r = 0$, so $\operatorname{Ind}_P^G(J^r) = 0$.

Now, splice these together:

$$\begin{aligned}
0 \to \operatorname{Ind}_P^G(J^{r-1}) &\to \operatorname{Ind}_P^G(J^{r-2}) \to \operatorname{Ind}_P^G(k)^{\oplus d_{r-2}} \to 0 \\
&\to \operatorname{Ind}_P^G(J^{r-3}) \to \operatorname{Ind}_P^G(k)^{\oplus d_{r-3}} \to 0 \\
&\cdots \\
&\to \operatorname{Ind}_P^G(J^0) = kG \to \operatorname{Ind}_P^G(k) \to 0
\end{aligned}$$

This gives a resolution of $\operatorname{Ind}_P^G(k)$ by modules $\operatorname{Ind}_P^G(k)^{\oplus d_i}$. But we want a resolution of the trivial module $k$, not of $\operatorname{Ind}_P^G(k)$.

So we need one more step: resolve $\operatorname{Ind}_P^G(k)$ in terms of $k$.

Since $p \nmid [G:P]$, $\operatorname{Ind}_P^G(k) \cong k \oplus M$ where $M$ is a complement. Let $\pi: \operatorname{Ind}_P^G(k) \to k$ be the projection.

Now apply the SAME trick to $M$: $M$ is a $kG$-module, hence relatively $P$-projective, so... no wait, we need $M$ to be induced from $P$ for the resolution to be by induced modules.

Hmm. The problem is that $M$ (the complement of $k$ in $\operatorname{Ind}_P^G(k)$) is NOT generally induced from $P$. So we can't just repeat the construction.

**Different idea:** Instead of resolving $\operatorname{Ind}_P^G(k)$ first and then getting to $k$, let's resolve $k$ directly using the radical filtration of $kP$ and then applying an appropriate functor.

Actually, let me reconsider. The map $\operatorname{Ind}_P^G(k) \to k$ (augmentation on cosets) is split surjective with kernel $M$. We have:
$$0 \to M \to \operatorname{Ind}_P^G(k) \to k \to 0$$

Now, $M$ restricted to $P$: $\operatorname{Res}_P^G(M)$. Since every $kG$-module is relatively $P$-projective, $M$ is a direct summand of $\operatorname{Ind}_P^G(\operatorname{Res}_P^G(M))$. But this doesn't express $M$ as induced from $P$.

**ALTERNATIVE CONSTRUCTION:**

What if we use the *complete* radical filtration of $kG$ as a $kG$-module, but relative to the $(G,P)$-relative projectivity? 

The idea: $kG$ is free over $kG$, so it's certainly induced from $P$ (it's $\operatorname{Ind}_P^G(kP)$). The radical filtration of $kG$ as a left $kG$-module might give us something useful.

But the radical of $kG$ is NOT nilpotent in general (for non-$p$-groups), so this won't give a finite resolution.

**OK, let me try a completely different angle.** Perhaps the resolution is:

$$0 \to kG \otimes_{kP} J^{r-1} \to \cdots \to kG \otimes_{kP} J \to kG \otimes_{kP} kP \to kG \otimes_{kP} k \to k \to 0$$

Wait, the last map $kG \otimes_{kP} k \to k$ is the augmentation on cosets divided by $[G:P]$, which is split. But the term before it is $kG \otimes_{kP} kP = kG$, and the map $kG \to kG \otimes_{kP} k$ is the natural projection. 

So the complex would be:
$$0 \to kG \otimes_{kP} J^{r-1} \to \cdots \to kG \otimes_{kP} J \to kG \to kG \otimes_{kP} k \to k \to 0$$

Is this exact? Let's check exactness at $kG$. The map $kG \to kG \otimes_{kP} k$ is given by $g \mapsto g \otimes 1$. Its kernel is $\{\sum a_g g : \sum a_g = 0\} = \Delta_G$, the augmentation ideal of $kG$. And the image of $kG \otimes_{kP} J \to kG$ is... well, $J$ is the augmentation ideal of $kP$, and under the map $kG \otimes_{kP} J \to kG$, we get the $kG$-submodule generated by $J$. 

The image of $kG \otimes_{kP} J \to kG$ is $kG \cdot J$, which is a proper submodule of $\Delta_G$ in general (unless $G$ is a $p$-group). So exactness fails at $kG$.

This approach doesn't work.

**Let me think about the problem statement literally.** "The trivial representation has a finite resolution by representations induced from its Sylow $p$-group."

Wait - maybe this is a known theorem about the existence of a "finite relative projective resolution" and it follows from the fact that the *relative global dimension* of $kG$ with respect to $P$ is finite. For a $p$-group $P$, the relative global dimension with respect to the trivial subgroup is infinite (since $kP$ is not semisimple). But with respect to $P$ itself? The relative global dimension of $kG$ with respect to $P$ might be the same as the global dimension of $kP$, which is infinite.

Hmm, but maybe for the trivial module specifically, things are different.

**WAIT - I just had an important insight.** Maybe the resolution is of length exactly 1! That is:

$$0 \to \operatorname{Ind}_P^G(V) \to \operatorname{Ind}_P^G(k) \to k \to 0$$

for some $kP$-module $V$. Is this possible?

For this to be exact, we need a $kP$-module $V$ such that $\operatorname{Ind}_P^G(V)$ is isomorphic to the kernel of $\operatorname{Ind}_P^G(k) \twoheadrightarrow k$. 

The kernel $M$ of $\operatorname{Ind}_P^G(k) \to k$ is the set of formal sums $\sum a_{gP} gP$ where $\sum a_{gP} = 0$. This is a $kG$-module of dimension $[G:P] - 1$.

If $M \cong \operatorname{Ind}_P^G(V)$ for some $V$, then $\dim V = \dim M / [G:P]$, which might not be an integer. For $G = S_3, p = 2$, $\dim M = 3-1 = 2$, $[G:P] = 3$, so $\dim V = 2/3$, impossible. So the resolution cannot have length 1.

What about length 2?
$$0 \to \operatorname{Ind}_P^G(V_2) \to \operatorname{Ind}_P^G(V_1) \to \operatorname{Ind}_P^G(V_0) \to k \to 0$$

The alternating sum of dimensions must be $1$ (the dimension of $k$):
$$[G:P](\dim V_2 - \dim V_1 + \dim V_0) = 1$$

So $[G:P]$ must divide 1, i.e., $[G:P] = 1$, meaning $G = P$. So for nontrivial $G$, we need more than 3 terms.

Hmm wait, the alternating sum should be:
$$\dim(\text{last term}) - \dim(\text{second-to-last}) + \cdots + (-1)^n \dim(\text{first}) = (-1)^n \dim(k) = (-1)^n$$

For a resolution of length $n+1$ (with $n+2$ terms including $k$):
$$\sum_{i=0}^n (-1)^i \dim I_i = (-1)^n$$

Each $I_i = \operatorname{Ind}_P^G(V_i)$, so $\dim I_i = [G:P] \cdot \dim V_i$. Thus:
$$[G:P] \cdot \sum_{i=0}^n (-1)^i \dim V_i = (-1)^n$$

This means $[G:P]$ divides $(-1)^n$, so $[G:P] = 1$. But $[G:P] \neq 1$ unless $G = P$.

**This seems to prove that no such finite resolution can exist, unless the terms are not direct sums of induced modules but some other construction.**

BUT WAIT — the terms don't have to be SINGLE induced modules. They can be DIRECT SUMS of induced modules. That doesn't change the dimension argument: each term is still of the form $\operatorname{Ind}_P^G(\text{something})$, so its dimension is divisible by $[G:P]$.

So if each term has dimension divisible by $[G:P]$, then $\sum (-1)^i \dim I_i$ is divisible by $[G:P]$. But this sum equals $\pm 1$ in absolute value. For this to hold, we need $[G:P] = 1$.

**This is a contradiction!** Unless I'm misunderstanding what "resolution" means here, or unless the maps are not $kG$-module maps (but surely they are, otherwise it's not a resolution in the module category).

Wait, maybe the resolution is NOT by modules but by "representations" in some looser sense? Or maybe it's an **injective** resolution (cohomological indexing) rather than projective?

Even for an injective resolution, the same dimension argument applies.

Actually wait, for an injective resolution (or any exact sequence of the form):
$$\cdots \to I_2 \to I_1 \to I_0 \to k \to 0$$
The alternating sum of dimensions equals $\dim k = 1$ if it's finite. So the same argument holds.

Hmm, OR does the resolution go the other way?
$$0 \to k \to I_0 \to I_1 \to \cdots \to I_n \to 0$$

For an exact sequence starting from $k$, the alternating sum of dimensions would be:
$$\dim k - \sum_{j=0}^n (-1)^j \dim I_j = 0$$
$$\dim k = \sum_{j=0}^n (-1)^j \dim I_j$$

Again, $\dim I_j$ is divisible by $[G:P]$, so the right side is divisible by $[G:P]$, but $\dim k = 1$. Contradiction.

**So the only way this can work is if the terms are NOT of the form $\operatorname{Ind}_P^G(V)$ but something else!**

Wait, let me re-read the problem: "representations induced from its Sylow $p$-group." What if "induced" here means something different? What if it means induced from $P$-modules but allowing submodules, quotients, and extensions?

Or what if the resolution uses arbitrary modules that are "induced" in the sense of being *relatively $P$-projective*? In that case, modules could have arbitrary dimensions.

But the phrase "representations induced from" typically means modules of the form $\operatorname{Ind}_P^G(V)$.

Unless... the representations themselves can be DIRECT SUMMANDS of induced modules. That's what "relatively $P$-projective" means. But then the dimension argument still holds if they're summands of induced modules.

HOLD ON. Let me reconsider. A module of the form $\operatorname{Ind}_P^G(V)$ has dimension $[G:P] \cdot \dim V$. This IS divisible by $[G:P]$. 

The alternating sum of any exact sequence of such modules would have total dimension divisible by $[G:P]$. But the alternating sum must equal $\dim(k) = 1$ for a resolution of $k$.

THIS CREATES A CONTRADICTION. So either:

1. The resolution is not an exact sequence of $kG$-modules, or
2. The terms are not restricted to being induced from $P$, or
3. I'm misunderstanding the meaning of "resolution", or
4. The resolution is not in the category of finite-dimensional modules? No, $kG$-modules from representations are finite-dimensional.

Actually wait — could it be that the resolution includes terms that are direct sums where each summand is induced from $P$, but different summands are induced from possibly *different* Sylow $p$-subgroups? The Sylow $p$-subgroups are all conjugate, so this doesn't change anything dimension-wise.

Let me reconsider the problem statement very carefully: "Prove that for $\overline{\mathbb{F}}_p$-representations of a finite group $G$, the trivial representation has a finite resolution by representations induced from its Sylow $p$-group."

I wonder if "Sylow $p$-group" refers to the Sylow $p$-subgroup of **a** finite group of Lie type in defining characteristic... The reference says: "For finite group of Lie type $G(\mathbb{F}_p)$, where $G$ is a split reductive group defined over $\mathbb{F}_p$ there are two cases: 1. $p \neq \ell$. 2. $p = \ell$ (called 'in defining characteristic')."

So the problem might be specifically about finite groups of Lie type in defining characteristic! The reference file is about modular representation theory for groups of Lie type, and the note about defining characteristic is relevant.

But the problem statement says "a finite group $G$," not a group of Lie type specifically. Hmm.

Let me re-read: "Prove that for $\overline{\mathbb{F}}_p$-representations of a finite group $G$, the trivial representation has a finite resolution by representations induced from its Sylow $p$-group."

This seems to be a general statement about any finite group $G$. The reference is just giving background on modular representation theory.

OK, let me question my dimension argument more carefully. An exact sequence of $kG$-modules:
$$0 \to I_n \xrightarrow{d_n} I_{n-1} \to \cdots \to I_0 \xrightarrow{d_0} k \to 0$$

The Euler characteristic: $\sum_{i=0}^n (-1)^i \dim I_i + (-1)^{n+1} = 0$? No, let me compute carefully.

For exactness at each step, the alternating sum of dimensions of all terms in an exact sequence equals 0. Here the sequence is:
$$0 \to I_n \to \cdots \to I_0 \to k \to 0$$

This is an exact sequence with $n+3$ terms including the zeros at the ends. The alternating sum:
$$\sum_{i=0}^n (-1)^i \dim I_i + (-1)^{n+1} \dim(k) + \sum_{\text{zeros}} 0 = 0$$

Wait, the standard formula: for an exact sequence $0 \to A_n \to A_{n-1} \to \cdots \to A_0 \to 0$, the alternating sum of dimensions $\sum (-1)^i \dim A_i = 0$.

So our sequence is:
$$0 \to I_n \to I_{n-1} \to \cdots \to I_0 \to k \to 0$$

This has $n+3$ terms (including the two zeros). The alternating sum:
$$(-1)^n \dim I_n + (-1)^{n-1} \dim I_{n-1} + \cdots + (-1)^0 \dim I_0 + (-1)^{-1} \dim k = 0$$

Wait, let me index properly. The sequence is:
$$0 \to A_{n+2} \to A_{n+1} \to \cdots \to A_0 \to 0$$
where $A_{n+2} = 0, A_{n+1} = I_n, \ldots, A_1 = I_0, A_0 = k$.

Alternating sum: $\sum_{i=0}^{n+2} (-1)^i \dim A_i = 0$.

So: $(-1)^0 \dim k + \sum_{i=1}^{n+1} (-1)^i \dim I_{i-1} + 0 = 0$.

That is: $\dim k + \sum_{j=0}^n (-1)^{j+1} \dim I_j = 0$.

So: $\dim k = \sum_{j=0}^n (-1)^j \dim I_j$.

If each $\dim I_j$ is divisible by $[G:P]$, then $\sum (-1)^j \dim I_j$ is divisible by $[G:P]$. But $\dim k = 1$. So $[G:P] \mid 1$, hence $[G:P] = 1$. Contradiction for nontrivial $G$.

**EXCEPT** — what if the sequence is indexed differently? What if the resolution is:
$$0 \to k \to I_0 \to I_1 \to \cdots \to I_n \to 0$$

Then $\dim k + \sum_{j=0}^n (-1)^{j+1} \dim I_j = 0$, so $\dim k = \sum_{j=0}^n (-1)^j \dim I_j$. Same contradiction.

**THIS SEEMS TO BE A GENUINE OBSTACLE.** Unless I'm wrong about dimensions of induced modules.

Let me double-check: $\operatorname{Ind}_P^G(V) = kG \otimes_{kP} V$. As a $k$-vector space, $\dim \operatorname{Ind}_P^G(V) = [G:P] \cdot \dim V$. Yes, this is correct because $kG$ is free of rank $[G:P]$ as a right $kP$-module.

So $\dim \operatorname{Ind}_P^G(V)$ is always divisible by $[G:P]$. And a direct sum of such modules also has dimension divisible by $[G:P]$. 

Therefore, the Euler characteristic of any complex of such modules is divisible by $[G:P]$. But the Euler characteristic of a resolution of $k$ must be $\dim k = 1$. This gives a contradiction unless $[G:P] = 1$.

**IS THE PROBLEM FALSE?** That can't be right for a problem posed this way.

Let me reconsider. Maybe "resolution" doesn't mean an exact sequence in the category of $kG$-modules. Maybe it means something like a *finite projective resolution in the relative stable category*, or a *resolution in the bounded derived category*, or...

Or maybe the statement is about something completely different. Let me re-read: "the trivial representation has a finite resolution by representations induced from its Sylow $p$-group."

What if by "resolution" they mean a filtration, not an exact sequence? Like a composition series where each factor is induced from $P$? That would make the dimension argument an identity rather than an alternating sum.

Or what if the resolution is in the **Grothendieck group**? Like, the trivial representation can be expressed as a $\mathbb{Z}$-linear combination of induced modules in the Grothendieck group $K_0(kG\text{-mod})$?

Actually, this interpretation makes a lot more sense! In modular representation theory, one often wants to express the class of the trivial module in $K_0(kG\text{-mod})$ as a linear combination of modules induced from a Sylow $p$-subgroup. This is related to the **Alperin weight conjecture** or the **Brauer's theorem on induced characters** in the modular setting.

But the word "resolution" strongly suggests an exact sequence, not just a virtual combination.

Let me reconsider once more. Maybe the resolution is in the **derived category** where we're allowed to use shifts? No, that would still have the same Euler characteristic issue.

**WAIT.** I've been assuming that the dimensions are taken over $k = \overline{\mathbb{F}}_p$. But what if "resolution" means something where we DON'T take alternating sums? For example, a **projective resolution in a Frobenius category** where the homotopy category matters, not the exact sequence?

No, that doesn't make sense.

Let me try yet another interpretation. Maybe the problem is about the trivial representation in the **stable module category** and the "resolution" is a sequence of maps that is exact in the stable category? In the stable category, dimension arguments don't apply in the same way.

But "finite resolution by representations" strongly suggests an ordinary exact sequence.

OK, let me try to find a flaw in my dimension argument. Is it possible that some $\operatorname{Ind}_P^G(V)$ has dimension NOT divisible by $[G:P]$?

$\operatorname{Ind}_P^G(V) = kG \otimes_{kP} V$. As a right $kP$-module, $kG$ is free of rank $[G:P]$. So as $k$-vector spaces, $kG \otimes_{kP} V \cong k^{[G:P]} \otimes_k V$, which has dimension $[G:P] \cdot \dim V$. Yes, the dimension IS divisible by $[G:P]$.

Unless $V$ is infinite-dimensional? But representations of a finite group over $\overline{\mathbb{F}}_p$ are finite-dimensional.

The only way to escape the contradiction is if the terms in the resolution are NOT of the form $\operatorname{Ind}_P^G(V)$ or direct sums thereof. What if "induced from its Sylow $p$-group" means something like: each term is a module that is **relatively $P$-projective**, i.e., a direct summand of a module induced from $P$?

A direct summand $X$ of $\operatorname{Ind}_P^G(V)$ does NOT necessarily have dimension divisible by $[G:P]$. For example, $k$ is a direct summand of $\operatorname{Ind}_P^G(k)$ with dimension 1, which is not divisible by $[G:P]$ (unless $[G:P]=1$).

So if the resolution uses relatively $P$-projective modules (i.e., modules that are direct summands of modules induced from $P$), the dimension argument doesn't force $[G:P]=1$.

But the problem says "representations **induced from** its Sylow $p$-group", not "relatively $P$-projective representations". However, in common mathematical usage, "induced" might be used loosely here.

**ALTERNATIVELY**, maybe "representations induced from its Sylow $p$-group" means representations that are **obtained by induction** from the Sylow $p$-group, but each step of induction could involve a different Sylow $p$-subgroup (they're all conjugate), and the resolution uses modules that are *summands* of such induced modules?

Actually, I think the most natural interpretation that makes the statement true is:

The trivial module has a finite **projective resolution** in the category of $\overline{\mathbb{F}}_p G$-modules by modules that are **$P$-projective** (i.e., relatively projective with respect to the Sylow $p$-subgroup $P$).

And a module is $P$-projective iff it is a direct summand of a module induced from $P$.

Now, is it true that the trivial module has a finite resolution by $P$-projective modules? Since EVERY $kG$-module is $P$-projective (because $p \nmid [G:P]$), the trivial module itself is $P$-projective, so we have a resolution of length 0!

$$0 \to k \xrightarrow{\operatorname{id}} k \to 0$$

But this is trivially a resolution — not interesting. The first two terms are $P$-projective (since $k$ is $P$-projective), so we're done.

Wait, but is $k$ (the trivial module) itself induced from $P$? No, $k$ has dimension 1, while $\operatorname{Ind}_P^G(V)$ has dimension $[G:P] \cdot \dim V$, so $k$ is induced from $P$ iff $[G:P]=1$ (i.e., $G=P$). But $k$ is relatively $P$-projective (a summand of an induced module), not necessarily induced.

Hmm, so if we interpret "resolution by representations induced from its Sylow $p$-group" as "resolution by $P$-projective modules," then a resolution of length 0 works, which is trivially true. This seems too trivial.

Maybe the problem is asking for something more specific. Let me think about what would be a nontrivial theorem.

What if the claim is: the trivial module has a finite resolution where each term is a **direct sum of modules of the form $\operatorname{Ind}_P^G(k)$** (i.e., permutation modules on $G/P$)?

In that case, each term has dimension divisible by $[G:P]$, and the Euler characteristic argument gives a contradiction unless $[G:P]=1$.

I'm stuck on the dimension argument. Let me verify it one more time with a concrete example.

Let $G = C_p \times C_q$ where $p, q$ are distinct primes. $P = C_p$ is a Sylow $p$-subgroup. $[G:P] = q$.

$\operatorname{Ind}_P^G(k)$ has dimension $q$. The trivial module $k$ has dimension 1. 

Can $k$ have a finite resolution by direct sums of $\operatorname{Ind}_P^G(V)$ for various $V$? 
Each term would have dimension divisible by $q$, so the Euler characteristic would be divisible by $q$. But the Euler characteristic of a resolution of $k$ is $\dim k = 1$. So $q \mid 1$, which means $q = 1$, i.e., $G = P$.

So for $G = C_p \times C_q$, NO such finite resolution exists by modules induced from $P$.

**This proves the statement as literally interpreted is FALSE.**

Unless I'm wrong about what "induced" means. In representation theory, $\operatorname{Ind}_H^G(V) = kG \otimes_{kH} V$ indeed has dimension $[G:H] \cdot \dim V$.

But wait — what about coinduced modules? $\operatorname{Coind}_H^G(V) = \operatorname{Hom}_{kH}(kG, V)$. For finite groups, $\operatorname{Coind}_H^G \cong \operatorname{Ind}_H^G$, so the dimension is the same.

What about twisted induction (like Harish-Chandra induction)? That's for algebraic groups, not finite groups.

What about "representations induced from its Sylow $p$-group" meaning something like: each term in the resolution is of the form $M \otimes_k N$ where one factor is induced from $P$? No, that doesn't match the phrasing.

Let me reconsider. Maybe I'm wrong that the categorical alternating sum of dimensions in an exact sequence equals $\dim k = 1$. Let me verify:

For an exact sequence $0 \to A \to B \to C \to 0$, $\dim A - \dim B + \dim C = 0$.

For $0 \to A \to B \to C \to D \to 0$, $\dim A - \dim B + \dim C - \dim D = 0$.

For $0 \to I_n \to \cdots \to I_0 \to k \to 0$ (resolution of $k$), we have $n+2$ terms (not counting the bookend zeros). The alternating sum is:
$$\dim I_n - \dim I_{n-1} + \cdots + (-1)^{n-1} \dim I_1 + (-1)^n \dim I_0 + (-1)^{n+1} \dim k = 0$$

So: $\dim k = \sum_{i=0}^n (-1)^i \dim I_{n-i}$ (depending on parity).

More simply, let $n$ be the length of the resolution (number of terms besides $k$). Then:
$$\dim k = \left|\sum_{i=0}^{n-1} (-1)^i \dim(\text{term}_i)\right|$$

For this to equal 1 while each term has dimension divisible by $[G:P]$, we need $[G:P]=1$.

So the literal statement is false for any $G \neq P$.

**BUT** — what if the problem means something else by "representations induced from its Sylow $p$-group"? What about the following interpretation:

"Representations induced from its Sylow $p$-group" means the resolution is built out of the *trivial representation induced from $P$* AND its tensor powers, or something?

Actually, what if "representations" is plural because there are multiple Sylow $p$-subgroups (all conjugate), and each term in the resolution is induced from the appropriate conjugate? But this doesn't change the dimension argument.

**NEW INTERPRETATION:** Maybe the resolution is NOT in the category of $kG$-modules but in the category of **$kG$-modules with a $P$-action** or something like that? Like, a resolution of the trivial representation by *relatively free* $kG$-modules?

Or maybe the claim is about the **trivial representation of $P$**, not of $G$? No, it says "the trivial representation" without qualification, which means the trivial representation of $G$.

Let me re-read the problem ONE MORE TIME: "Prove that for $\overline{\mathbb{F}}_p$-representations of a finite group $G$, the trivial representation has a finite resolution by representations induced from its Sylow $p$-group."

I wonder if "its" refers to the trivial representation's Sylow $p$-group... no, "its" refers to $G$'s Sylow $p$-group. Or does it? "its" could grammatically refer to "the trivial representation"! But the trivial representation of a group doesn't have a Sylow $p$-group; only the group $G$ does.

OK, I'm going to assume the intended interpretation is that each term in the resolution is a **relatively $P$-projective module** (a direct summand of a module induced from $P$), and the proof uses the fact that every $kG$-module is relatively $P$-projective when $P$ is a Sylow $p$-subgroup. In that case, the trivial module $k$ is $P$-projective, so we can take the **relative projective resolution**:

The relative bar resolution gives an (infinite) resolution of $k$ by $P$-projective modules. But each $P$-projective module is a summand of a module induced from $P$. The resolution might not be finite though.

Hmm. Unless there's a way to make the relative projective resolution finite...

ACTUALLY, now I recall something important. In **relative homological algebra**, for a finite group $G$ and $P$ a Sylow $p$-subgroup, the **relative global dimension** of $kG$ with respect to $P$ is finite. Wait, is it?

I know that for a $p$-group $P$, the global dimension of $kP$ is infinite (since $kP$ is not semisimple unless $P$ is trivial). And the relative global dimension of $kG$ with respect to $P$ might equal the global dimension of $kP$ in many cases... Let me think.

Actually, there's a theorem: if $kG$ is an $H$-separable extension of $kP$ (which is true since $p \nmid [G:P]$), then the relative global dimension of $kG$ with respect to $P$ equals the global dimension of $kP$. Since $\operatorname{gl.dim}(kP) = \infty$ (for nontrivial $P$), the relative global dimension is also infinite, meaning that there exist modules whose relative projective resolutions are infinite.

But the question is specifically about the **trivial** module. Maybe the trivial module has a finite relative projective resolution even if other modules don't?

Let me compute for a simple example. Take $G = C_p \times C_q$ with $p \neq q$, $P = C_p$, $k = \overline{\mathbb{F}}_p$.

The trivial $kG$-module is $k$ (dimension 1). The relative projective resolution starts:
$$\cdots \to \operatorname{Ind}_P^G(\operatorname{Res}_P^G(\Omega^1(k))) \to \operatorname{Ind}_P^G(\operatorname{Res}_P^G(k)) \to k \to 0$$

$\operatorname{Res}_P^G(k) = k$, so $\operatorname{Ind}_P^G(k) = k[G/P]$. This has dimension $q$. The map $\operatorname{Ind}_P^G(k) \to k$ (augmentation divided by $q$) has kernel of dimension $q-1$. 

Now we need the relative syzygy: $\Omega_{G/P}(k) = \ker(\operatorname{Ind}_P^G(\operatorname{Res}_P^G(k)) \to k)$. This has dimension $q-1$.

Next, $\operatorname{Res}_P^G(\Omega_{G/P}(k))$ is $\Omega_{G/P}(k
tokens used
35,341
arXiv API keeps timing out. Let me shift to pure mathematical reasoning and construct the proof from first principles. I know enough representation theory to do this.

Let me think deeply about what the correct proof approach is.

**Rethinking from scratch:**

The problem: $G$ finite group, $P \in \text{Syl}_p(G)$, $k = \overline{\mathbb{F}}_p$. Show $\mathbf{1}_k$ has a finite resolution by modules $\operatorname{Ind}_P^G(V)$.

The key mathematical structure that gives a finite resolution is the **radical series of the group algebra $kP$ of the $p$-group $P$**.

Since $P$ is a $p$-group and $\operatorname{char}(k) = p$, the group algebra $kP$ is a local ring with unique maximal ideal $J = \operatorname{rad}(kP)$, which equals the augmentation ideal $\Delta_P = \ker(kP \to k)$. The algebra $kP$ is Artinian and $J$ is nilpotent: $J^r = 0$ for some $r$.

Now, consider the short exact sequence of $kP$-modules:
$$0 \to J \to kP \to k \to 0$$

Apply the exact functor $\operatorname{Ind}_P^G = kG \otimes_{kP} -$:
$$0 \to \operatorname{Ind}_P^G(J) \to \operatorname{Ind}_P^G(kP) \to \operatorname{Ind}_P^G(k) \to 0$$

Note $\operatorname{Ind}_P^G(kP) = kG \otimes_{kP} kP = kG$.

Now, $\operatorname{Ind}_P^G(k) = k[G/P]$ has the trivial module as a direct summand. The map $\operatorname{Ind}_P^G(kP) = kG \to \operatorname{Ind}_P^G(k) = k[G/P]$ sends $g$ to $gP$. The augmentation $\epsilon: k[G/P] \to k$ sends all cosets to 1. Compose: $kG \to k[G/P] \to k$ gives the ordinary augmentation $kG \to k$.

Now, the radical filtration of $kP$ as a left $kP$-module:
$$kP = J^0 \supset J^1 \supset J^2 \supset \cdots \supset J^r = 0$$

Each subquotient $J^i/J^{i+1}$ is a semisimple $kP$-module. Since $kP$ is a local algebra whose only simple module is $k$, each $J^i/J^{i+1} \cong k^{\oplus d_i}$ for some $d_i \geq 0$.

Now, the key idea: **build a resolution from these subquotients by splicing.**

Consider the short exact sequences (for $i = 0, 1, \ldots, r-1$):
$$0 \to J^{i+1} \to J^i \to k^{\oplus d_i} \to 0$$

Apply $\operatorname{Ind}_P^G$:
$$0 \to \operatorname{Ind}_P^G(J^{i+1}) \to \operatorname{Ind}_P^G(J^i) \to \operatorname{Ind}_P^G(k)^{\oplus d_i} \to 0$$

All terms are induced from $P$! And $J^r = 0$, so $\operatorname{Ind}_P^G(J^r) = 0$.

Now, splice these together:

$$\begin{aligned}
0 \to \operatorname{Ind}_P^G(J^{r-1}) &\to \operatorname{Ind}_P^G(J^{r-2}) \to \operatorname{Ind}_P^G(k)^{\oplus d_{r-2}} \to 0 \\
&\to \operatorname{Ind}_P^G(J^{r-3}) \to \operatorname{Ind}_P^G(k)^{\oplus d_{r-3}} \to 0 \\
&\cdots \\
&\to \operatorname{Ind}_P^G(J^0) = kG \to \operatorname{Ind}_P^G(k) \to 0
\end{aligned}$$

This gives a resolution of $\operatorname{Ind}_P^G(k)$ by modules $\operatorname{Ind}_P^G(k)^{\oplus d_i}$. But we want a resolution of the trivial module $k$, not of $\operatorname{Ind}_P^G(k)$.

So we need one more step: resolve $\operatorname{Ind}_P^G(k)$ in terms of $k$.

Since $p \nmid [G:P]$, $\operatorname{Ind}_P^G(k) \cong k \oplus M$ where $M$ is a complement. Let $\pi: \operatorname{Ind}_P^G(k) \to k$ be the projection.

Now apply the SAME trick to $M$: $M$ is a $kG$-module, hence relatively $P$-projective, so... no wait, we need $M$ to be induced from $P$ for the resolution to be by induced modules.

Hmm. The problem is that $M$ (the complement of $k$ in $\operatorname{Ind}_P^G(k)$) is NOT generally induced from $P$. So we can't just repeat the construction.

**Different idea:** Instead of resolving $\operatorname{Ind}_P^G(k)$ first and then getting to $k$, let's resolve $k$ directly using the radical filtration of $kP$ and then applying an appropriate functor.

Actually, let me reconsider. The map $\operatorname{Ind}_P^G(k) \to k$ (augmentation on cosets) is split surjective with kernel $M$. We have:
$$0 \to M \to \operatorname{Ind}_P^G(k) \to k \to 0$$

Now, $M$ restricted to $P$: $\operatorname{Res}_P^G(M)$. Since every $kG$-module is relatively $P$-projective, $M$ is a direct summand of $\operatorname{Ind}_P^G(\operatorname{Res}_P^G(M))$. But this doesn't express $M$ as induced from $P$.

**ALTERNATIVE CONSTRUCTION:**

What if we use the *complete* radical filtration of $kG$ as a $kG$-module, but relative to the $(G,P)$-relative projectivity? 

The idea: $kG$ is free over $kG$, so it's certainly induced from $P$ (it's $\operatorname{Ind}_P^G(kP)$). The radical filtration of $kG$ as a left $kG$-module might give us something useful.

But the radical of $kG$ is NOT nilpotent in general (for non-$p$-groups), so this won't give a finite resolution.

**OK, let me try a completely different angle.** Perhaps the resolution is:

$$0 \to kG \otimes_{kP} J^{r-1} \to \cdots \to kG \otimes_{kP} J \to kG \otimes_{kP} kP \to kG \otimes_{kP} k \to k \to 0$$

Wait, the last map $kG \otimes_{kP} k \to k$ is the augmentation on cosets divided by $[G:P]$, which is split. But the term before it is $kG \otimes_{kP} kP = kG$, and the map $kG \to kG \otimes_{kP} k$ is the natural projection. 

So the complex would be:
$$0 \to kG \otimes_{kP} J^{r-1} \to \cdots \to kG \otimes_{kP} J \to kG \to kG \otimes_{kP} k \to k \to 0$$

Is this exact? Let's check exactness at $kG$. The map $kG \to kG \otimes_{kP} k$ is given by $g \mapsto g \otimes 1$. Its kernel is $\{\sum a_g g : \sum a_g = 0\} = \Delta_G$, the augmentation ideal of $kG$. And the image of $kG \otimes_{kP} J \to kG$ is... well, $J$ is the augmentation ideal of $kP$, and under the map $kG \otimes_{kP} J \to kG$, we get the $kG$-submodule generated by $J$. 

The image of $kG \otimes_{kP} J \to kG$ is $kG \cdot J$, which is a proper submodule of $\Delta_G$ in general (unless $G$ is a $p$-group). So exactness fails at $kG$.

This approach doesn't work.

**Let me think about the problem statement literally.** "The trivial representation has a finite resolution by representations induced from its Sylow $p$-group."

Wait - maybe this is a known theorem about the existence of a "finite relative projective resolution" and it follows from the fact that the *relative global dimension* of $kG$ with respect to $P$ is finite. For a $p$-group $P$, the relative global dimension with respect to the trivial subgroup is infinite (since $kP$ is not semisimple). But with respect to $P$ itself? The relative global dimension of $kG$ with respect to $P$ might be the same as the global dimension of $kP$, which is infinite.

Hmm, but maybe for the trivial module specifically, things are different.

**WAIT - I just had an important insight.** Maybe the resolution is of length exactly 1! That is:

$$0 \to \operatorname{Ind}_P^G(V) \to \operatorname{Ind}_P^G(k) \to k \to 0$$

for some $kP$-module $V$. Is this possible?

For this to be exact, we need a $kP$-module $V$ such that $\operatorname{Ind}_P^G(V)$ is isomorphic to the kernel of $\operatorname{Ind}_P^G(k) \twoheadrightarrow k$. 

The kernel $M$ of $\operatorname{Ind}_P^G(k) \to k$ is the set of formal sums $\sum a_{gP} gP$ where $\sum a_{gP} = 0$. This is a $kG$-module of dimension $[G:P] - 1$.

If $M \cong \operatorname{Ind}_P^G(V)$ for some $V$, then $\dim V = \dim M / [G:P]$, which might not be an integer. For $G = S_3, p = 2$, $\dim M = 3-1 = 2$, $[G:P] = 3$, so $\dim V = 2/3$, impossible. So the resolution cannot have length 1.

What about length 2?
$$0 \to \operatorname{Ind}_P^G(V_2) \to \operatorname{Ind}_P^G(V_1) \to \operatorname{Ind}_P^G(V_0) \to k \to 0$$

The alternating sum of dimensions must be $1$ (the dimension of $k$):
$$[G:P](\dim V_2 - \dim V_1 + \dim V_0) = 1$$

So $[G:P]$ must divide 1, i.e., $[G:P] = 1$, meaning $G = P$. So for nontrivial $G$, we need more than 3 terms.

Hmm wait, the alternating sum should be:
$$\dim(\text{last term}) - \dim(\text{second-to-last}) + \cdots + (-1)^n \dim(\text{first}) = (-1)^n \dim(k) = (-1)^n$$

For a resolution of length $n+1$ (with $n+2$ terms including $k$):
$$\sum_{i=0}^n (-1)^i \dim I_i = (-1)^n$$

Each $I_i = \operatorname{Ind}_P^G(V_i)$, so $\dim I_i = [G:P] \cdot \dim V_i$. Thus:
$$[G:P] \cdot \sum_{i=0}^n (-1)^i \dim V_i = (-1)^n$$

This means $[G:P]$ divides $(-1)^n$, so $[G:P] = 1$. But $[G:P] \neq 1$ unless $G = P$.

**This seems to prove that no such finite resolution can exist, unless the terms are not direct sums of induced modules but some other construction.**

BUT WAIT — the terms don't have to be SINGLE induced modules. They can be DIRECT SUMS of induced modules. That doesn't change the dimension argument: each term is still of the form $\operatorname{Ind}_P^G(\text{something})$, so its dimension is divisible by $[G:P]$.

So if each term has dimension divisible by $[G:P]$, then $\sum (-1)^i \dim I_i$ is divisible by $[G:P]$. But this sum equals $\pm 1$ in absolute value. For this to hold, we need $[G:P] = 1$.

**This is a contradiction!** Unless I'm misunderstanding what "resolution" means here, or unless the maps are not $kG$-module maps (but surely they are, otherwise it's not a resolution in the module category).

Wait, maybe the resolution is NOT by modules but by "representations" in some looser sense? Or maybe it's an **injective** resolution (cohomological indexing) rather than projective?

Even for an injective resolution, the same dimension argument applies.

Actually wait, for an injective resolution (or any exact sequence of the form):
$$\cdots \to I_2 \to I_1 \to I_0 \to k \to 0$$
The alternating sum of dimensions equals $\dim k = 1$ if it's finite. So the same argument holds.

Hmm, OR does the resolution go the other way?
$$0 \to k \to I_0 \to I_1 \to \cdots \to I_n \to 0$$

For an exact sequence starting from $k$, the alternating sum of dimensions would be:
$$\dim k - \sum_{j=0}^n (-1)^j \dim I_j = 0$$
$$\dim k = \sum_{j=0}^n (-1)^j \dim I_j$$

Again, $\dim I_j$ is divisible by $[G:P]$, so the right side is divisible by $[G:P]$, but $\dim k = 1$. Contradiction.

**So the only way this can work is if the terms are NOT of the form $\operatorname{Ind}_P^G(V)$ but something else!**

Wait, let me re-read the problem: "representations induced from its Sylow $p$-group." What if "induced" here means something different? What if it means induced from $P$-modules but allowing submodules, quotients, and extensions?

Or what if the resolution uses arbitrary modules that are "induced" in the sense of being *relatively $P$-projective*? In that case, modules could have arbitrary dimensions.

But the phrase "representations induced from" typically means modules of the form $\operatorname{Ind}_P^G(V)$.

Unless... the representations themselves can be DIRECT SUMMANDS of induced modules. That's what "relatively $P$-projective" means. But then the dimension argument still holds if they're summands of induced modules.

HOLD ON. Let me reconsider. A module of the form $\operatorname{Ind}_P^G(V)$ has dimension $[G:P] \cdot \dim V$. This IS divisible by $[G:P]$. 

The alternating sum of any exact sequence of such modules would have total dimension divisible by $[G:P]$. But the alternating sum must equal $\dim(k) = 1$ for a resolution of $k$.

THIS CREATES A CONTRADICTION. So either:

1. The resolution is not an exact sequence of $kG$-modules, or
2. The terms are not restricted to being induced from $P$, or
3. I'm misunderstanding the meaning of "resolution", or
4. The resolution is not in the category of finite-dimensional modules? No, $kG$-modules from representations are finite-dimensional.

Actually wait — could it be that the resolution includes terms that are direct sums where each summand is induced from $P$, but different summands are induced from possibly *different* Sylow $p$-subgroups? The Sylow $p$-subgroups are all conjugate, so this doesn't change anything dimension-wise.

Let me reconsider the problem statement very carefully: "Prove that for $\overline{\mathbb{F}}_p$-representations of a finite group $G$, the trivial representation has a finite resolution by representations induced from its Sylow $p$-group."

I wonder if "Sylow $p$-group" refers to the Sylow $p$-subgroup of **a** finite group of Lie type in defining characteristic... The reference says: "For finite group of Lie type $G(\mathbb{F}_p)$, where $G$ is a split reductive group defined over $\mathbb{F}_p$ there are two cases: 1. $p \neq \ell$. 2. $p = \ell$ (called 'in defining characteristic')."

So the problem might be specifically about finite groups of Lie type in defining characteristic! The reference file is about modular representation theory for groups of Lie type, and the note about defining characteristic is relevant.

But the problem statement says "a finite group $G$," not a group of Lie type specifically. Hmm.

Let me re-read: "Prove that for $\overline{\mathbb{F}}_p$-representations of a finite group $G$, the trivial representation has a finite resolution by representations induced from its Sylow $p$-group."

This seems to be a general statement about any finite group $G$. The reference is just giving background on modular representation theory.

OK, let me question my dimension argument more carefully. An exact sequence of $kG$-modules:
$$0 \to I_n \xrightarrow{d_n} I_{n-1} \to \cdots \to I_0 \xrightarrow{d_0} k \to 0$$

The Euler characteristic: $\sum_{i=0}^n (-1)^i \dim I_i + (-1)^{n+1} = 0$? No, let me compute carefully.

For exactness at each step, the alternating sum of dimensions of all terms in an exact sequence equals 0. Here the sequence is:
$$0 \to I_n \to \cdots \to I_0 \to k \to 0$$

This is an exact sequence with $n+3$ terms including the zeros at the ends. The alternating sum:
$$\sum_{i=0}^n (-1)^i \dim I_i + (-1)^{n+1} \dim(k) + \sum_{\text{zeros}} 0 = 0$$

Wait, the standard formula: for an exact sequence $0 \to A_n \to A_{n-1} \to \cdots \to A_0 \to 0$, the alternating sum of dimensions $\sum (-1)^i \dim A_i = 0$.

So our sequence is:
$$0 \to I_n \to I_{n-1} \to \cdots \to I_0 \to k \to 0$$

This has $n+3$ terms (including the two zeros). The alternating sum:
$$(-1)^n \dim I_n + (-1)^{n-1} \dim I_{n-1} + \cdots + (-1)^0 \dim I_0 + (-1)^{-1} \dim k = 0$$

Wait, let me index properly. The sequence is:
$$0 \to A_{n+2} \to A_{n+1} \to \cdots \to A_0 \to 0$$
where $A_{n+2} = 0, A_{n+1} = I_n, \ldots, A_1 = I_0, A_0 = k$.

Alternating sum: $\sum_{i=0}^{n+2} (-1)^i \dim A_i = 0$.

So: $(-1)^0 \dim k + \sum_{i=1}^{n+1} (-1)^i \dim I_{i-1} + 0 = 0$.

That is: $\dim k + \sum_{j=0}^n (-1)^{j+1} \dim I_j = 0$.

So: $\dim k = \sum_{j=0}^n (-1)^j \dim I_j$.

If each $\dim I_j$ is divisible by $[G:P]$, then $\sum (-1)^j \dim I_j$ is divisible by $[G:P]$. But $\dim k = 1$. So $[G:P] \mid 1$, hence $[G:P] = 1$. Contradiction for nontrivial $G$.

**EXCEPT** — what if the sequence is indexed differently? What if the resolution is:
$$0 \to k \to I_0 \to I_1 \to \cdots \to I_n \to 0$$

Then $\dim k + \sum_{j=0}^n (-1)^{j+1} \dim I_j = 0$, so $\dim k = \sum_{j=0}^n (-1)^j \dim I_j$. Same contradiction.

**THIS SEEMS TO BE A GENUINE OBSTACLE.** Unless I'm wrong about dimensions of induced modules.

Let me double-check: $\operatorname{Ind}_P^G(V) = kG \otimes_{kP} V$. As a $k$-vector space, $\dim \operatorname{Ind}_P^G(V) = [G:P] \cdot \dim V$. Yes, this is correct because $kG$ is free of rank $[G:P]$ as a right $kP$-module.

So $\dim \operatorname{Ind}_P^G(V)$ is always divisible by $[G:P]$. And a direct sum of such modules also has dimension divisible by $[G:P]$. 

Therefore, the Euler characteristic of any complex of such modules is divisible by $[G:P]$. But the Euler characteristic of a resolution of $k$ must be $\dim k = 1$. This gives a contradiction unless $[G:P] = 1$.

**IS THE PROBLEM FALSE?** That can't be right for a problem posed this way.

Let me reconsider. Maybe "resolution" doesn't mean an exact sequence in the category of $kG$-modules. Maybe it means something like a *finite projective resolution in the relative stable category*, or a *resolution in the bounded derived category*, or...

Or maybe the statement is about something completely different. Let me re-read: "the trivial representation has a finite resolution by representations induced from its Sylow $p$-group."

What if by "resolution" they mean a filtration, not an exact sequence? Like a composition series where each factor is induced from $P$? That would make the dimension argument an identity rather than an alternating sum.

Or what if the resolution is in the **Grothendieck group**? Like, the trivial representation can be expressed as a $\mathbb{Z}$-linear combination of induced modules in the Grothendieck group $K_0(kG\text{-mod})$?

Actually, this interpretation makes a lot more sense! In modular representation theory, one often wants to express the class of the trivial module in $K_0(kG\text{-mod})$ as a linear combination of modules induced from a Sylow $p$-subgroup. This is related to the **Alperin weight conjecture** or the **Brauer's theorem on induced characters** in the modular setting.

But the word "resolution" strongly suggests an exact sequence, not just a virtual combination.

Let me reconsider once more. Maybe the resolution is in the **derived category** where we're allowed to use shifts? No, that would still have the same Euler characteristic issue.

**WAIT.** I've been assuming that the dimensions are taken over $k = \overline{\mathbb{F}}_p$. But what if "resolution" means something where we DON'T take alternating sums? For example, a **projective resolution in a Frobenius category** where the homotopy category matters, not the exact sequence?

No, that doesn't make sense.

Let me try yet another interpretation. Maybe the problem is about the trivial representation in the **stable module category** and the "resolution" is a sequence of maps that is exact in the stable category? In the stable category, dimension arguments don't apply in the same way.

But "finite resolution by representations" strongly suggests an ordinary exact sequence.

OK, let me try to find a flaw in my dimension argument. Is it possible that some $\operatorname{Ind}_P^G(V)$ has dimension NOT divisible by $[G:P]$?

$\operatorname{Ind}_P^G(V) = kG \otimes_{kP} V$. As a right $kP$-module, $kG$ is free of rank $[G:P]$. So as $k$-vector spaces, $kG \otimes_{kP} V \cong k^{[G:P]} \otimes_k V$, which has dimension $[G:P] \cdot \dim V$. Yes, the dimension IS divisible by $[G:P]$.

Unless $V$ is infinite-dimensional? But representations of a finite group over $\overline{\mathbb{F}}_p$ are finite-dimensional.

The only way to escape the contradiction is if the terms in the resolution are NOT of the form $\operatorname{Ind}_P^G(V)$ or direct sums thereof. What if "induced from its Sylow $p$-group" means something like: each term is a module that is **relatively $P$-projective**, i.e., a direct summand of a module induced from $P$?

A direct summand $X$ of $\operatorname{Ind}_P^G(V)$ does NOT necessarily have dimension divisible by $[G:P]$. For example, $k$ is a direct summand of $\operatorname{Ind}_P^G(k)$ with dimension 1, which is not divisible by $[G:P]$ (unless $[G:P]=1$).

So if the resolution uses relatively $P$-projective modules (i.e., modules that are direct summands of modules induced from $P$), the dimension argument doesn't force $[G:P]=1$.

But the problem says "representations **induced from** its Sylow $p$-group", not "relatively $P$-projective representations". However, in common mathematical usage, "induced" might be used loosely here.

**ALTERNATIVELY**, maybe "representations induced from its Sylow $p$-group" means representations that are **obtained by induction** from the Sylow $p$-group, but each step of induction could involve a different Sylow $p$-subgroup (they're all conjugate), and the resolution uses modules that are *summands* of such induced modules?

Actually, I think the most natural interpretation that makes the statement true is:

The trivial module has a finite **projective resolution** in the category of $\overline{\mathbb{F}}_p G$-modules by modules that are **$P$-projective** (i.e., relatively projective with respect to the Sylow $p$-subgroup $P$).

And a module is $P$-projective iff it is a direct summand of a module induced from $P$.

Now, is it true that the trivial module has a finite resolution by $P$-projective modules? Since EVERY $kG$-module is $P$-projective (because $p \nmid [G:P]$), the trivial module itself is $P$-projective, so we have a resolution of length 0!

$$0 \to k \xrightarrow{\operatorname{id}} k \to 0$$

But this is trivially a resolution — not interesting. The first two terms are $P$-projective (since $k$ is $P$-projective), so we're done.

Wait, but is $k$ (the trivial module) itself induced from $P$? No, $k$ has dimension 1, while $\operatorname{Ind}_P^G(V)$ has dimension $[G:P] \cdot \dim V$, so $k$ is induced from $P$ iff $[G:P]=1$ (i.e., $G=P$). But $k$ is relatively $P$-projective (a summand of an induced module), not necessarily induced.

Hmm, so if we interpret "resolution by representations induced from its Sylow $p$-group" as "resolution by $P$-projective modules," then a resolution of length 0 works, which is trivially true. This seems too trivial.

Maybe the problem is asking for something more specific. Let me think about what would be a nontrivial theorem.

What if the claim is: the trivial module has a finite resolution where each term is a **direct sum of modules of the form $\operatorname{Ind}_P^G(k)$** (i.e., permutation modules on $G/P$)?

In that case, each term has dimension divisible by $[G:P]$, and the Euler characteristic argument gives a contradiction unless $[G:P]=1$.

I'm stuck on the dimension argument. Let me verify it one more time with a concrete example.

Let $G = C_p \times C_q$ where $p, q$ are distinct primes. $P = C_p$ is a Sylow $p$-subgroup. $[G:P] = q$.

$\operatorname{Ind}_P^G(k)$ has dimension $q$. The trivial module $k$ has dimension 1. 

Can $k$ have a finite resolution by direct sums of $\operatorname{Ind}_P^G(V)$ for various $V$? 
Each term would have dimension divisible by $q$, so the Euler characteristic would be divisible by $q$. But the Euler characteristic of a resolution of $k$ is $\dim k = 1$. So $q \mid 1$, which means $q = 1$, i.e., $G = P$.

So for $G = C_p \times C_q$, NO such finite resolution exists by modules induced from $P$.

**This proves the statement as literally interpreted is FALSE.**

Unless I'm wrong about what "induced" means. In representation theory, $\operatorname{Ind}_H^G(V) = kG \otimes_{kH} V$ indeed has dimension $[G:H] \cdot \dim V$.

But wait — what about coinduced modules? $\operatorname{Coind}_H^G(V) = \operatorname{Hom}_{kH}(kG, V)$. For finite groups, $\operatorname{Coind}_H^G \cong \operatorname{Ind}_H^G$, so the dimension is the same.

What about twisted induction (like Harish-Chandra induction)? That's for algebraic groups, not finite groups.

What about "representations induced from its Sylow $p$-group" meaning something like: each term in the resolution is of the form $M \otimes_k N$ where one factor is induced from $P$? No, that doesn't match the phrasing.

Let me reconsider. Maybe I'm wrong that the categorical alternating sum of dimensions in an exact sequence equals $\dim k = 1$. Let me verify:

For an exact sequence $0 \to A \to B \to C \to 0$, $\dim A - \dim B + \dim C = 0$.

For $0 \to A \to B \to C \to D \to 0$, $\dim A - \dim B + \dim C - \dim D = 0$.

For $0 \to I_n \to \cdots \to I_0 \to k \to 0$ (resolution of $k$), we have $n+2$ terms (not counting the bookend zeros). The alternating sum is:
$$\dim I_n - \dim I_{n-1} + \cdots + (-1)^{n-1} \dim I_1 + (-1)^n \dim I_0 + (-1)^{n+1} \dim k = 0$$

So: $\dim k = \sum_{i=0}^n (-1)^i \dim I_{n-i}$ (depending on parity).

More simply, let $n$ be the length of the resolution (number of terms besides $k$). Then:
$$\dim k = \left|\sum_{i=0}^{n-1} (-1)^i \dim(\text{term}_i)\right|$$

For this to equal 1 while each term has dimension divisible by $[G:P]$, we need $[G:P]=1$.

So the literal statement is false for any $G \neq P$.

**BUT** — what if the problem means something else by "representations induced from its Sylow $p$-group"? What about the following interpretation:

"Representations induced from its Sylow $p$-group" means the resolution is built out of the *trivial representation induced from $P$* AND its tensor powers, or something?

Actually, what if "representations" is plural because there are multiple Sylow $p$-subgroups (all conjugate), and each term in the resolution is induced from the appropriate conjugate? But this doesn't change the dimension argument.

**NEW INTERPRETATION:** Maybe the resolution is NOT in the category of $kG$-modules but in the category of **$kG$-modules with a $P$-action** or something like that? Like, a resolution of the trivial representation by *relatively free* $kG$-modules?

Or maybe the claim is about the **trivial representation of $P$**, not of $G$? No, it says "the trivial representation" without qualification, which means the trivial representation of $G$.

Let me re-read the problem ONE MORE TIME: "Prove that for $\overline{\mathbb{F}}_p$-representations of a finite group $G$, the trivial representation has a finite resolution by representations induced from its Sylow $p$-group."

I wonder if "its" refers to the trivial representation's Sylow $p$-group... no, "its" refers to $G$'s Sylow $p$-group. Or does it? "its" could grammatically refer to "the trivial representation"! But the trivial representation of a group doesn't have a Sylow $p$-group; only the group $G$ does.

OK, I'm going to assume the intended interpretation is that each term in the resolution is a **relatively $P$-projective module** (a direct summand of a module induced from $P$), and the proof uses the fact that every $kG$-module is relatively $P$-projective when $P$ is a Sylow $p$-subgroup. In that case, the trivial module $k$ is $P$-projective, so we can take the **relative projective resolution**:

The relative bar resolution gives an (infinite) resolution of $k$ by $P$-projective modules. But each $P$-projective module is a summand of a module induced from $P$. The resolution might not be finite though.

Hmm. Unless there's a way to make the relative projective resolution finite...

ACTUALLY, now I recall something important. In **relative homological algebra**, for a finite group $G$ and $P$ a Sylow $p$-subgroup, the **relative global dimension** of $kG$ with respect to $P$ is finite. Wait, is it?

I know that for a $p$-group $P$, the global dimension of $kP$ is infinite (since $kP$ is not semisimple unless $P$ is trivial). And the relative global dimension of $kG$ with respect to $P$ might equal the global dimension of $kP$ in many cases... Let me think.

Actually, there's a theorem: if $kG$ is an $H$-separable extension of $kP$ (which is true since $p \nmid [G:P]$), then the relative global dimension of $kG$ with respect to $P$ equals the global dimension of $kP$. Since $\operatorname{gl.dim}(kP) = \infty$ (for nontrivial $P$), the relative global dimension is also infinite, meaning that there exist modules whose relative projective resolutions are infinite.

But the question is specifically about the **trivial** module. Maybe the trivial module has a finite relative projective resolution even if other modules don't?

Let me compute for a simple example. Take $G = C_p \times C_q$ with $p \neq q$, $P = C_p$, $k = \overline{\mathbb{F}}_p$.

The trivial $kG$-module is $k$ (dimension 1). The relative projective resolution starts:
$$\cdots \to \operatorname{Ind}_P^G(\operatorname{Res}_P^G(\Omega^1(k))) \to \operatorname{Ind}_P^G(\operatorname{Res}_P^G(k)) \to k \to 0$$

$\operatorname{Res}_P^G(k) = k$, so $\operatorname{Ind}_P^G(k) = k[G/P]$. This has dimension $q$. The map $\operatorname{Ind}_P^G(k) \to k$ (augmentation divided by $q$) has kernel of dimension $q-1$. 

Now we need the relative syzygy: $\Omega_{G/P}(k) = \ker(\operatorname{Ind}_P^G(\operatorname{Res}_P^G(k)) \to k)$. This has dimension $q-1$.

Next, $\operatorname{Res}_P^G(\Omega_{G/P}(k))$ is $\Omega_{G/P}(k
