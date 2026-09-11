
# 1. Identity Verification Service

This a genuinely different problem domain from misinformation detection — it's closer to trust/credentialing infrastructure — but it's not unreasonable as an adjacent service, especially since "is this person really affiliated with this university?" is itself a common misinformation/impersonation vector (fake experts, fake credentials being cited to lend false authority). So there's a real connection to our mission. A few things worth thinking through before we scope it:

What "verification" can realistically mean. We have a spectrum of options, from lightweight to heavyweight:

## 1.1. **Domain/email-based verification (easiest, most defensible)**  

Most universities issue `@university.dz`-style institutional emails. A service where someone enters a claimed name + institutional email, we send a confirmation link to that email, and it confirms "this email address exists and is controlled by this person" is simple, low-risk, and doesn't require us to hold sensitive personal data. This is basically how services like ORCID or academic Slack communities do lightweight verification. It doesn't prove identity in a legal sense, but it proves institutional affiliation, which is usually what people actually want to know.

## 1.2. **Institutional directory lookups (harder, more useful)**  

If universities publish (or we can partner with them to access) staff/student directories, we could build a lookup tool: "is there a professor named X in department Y at university Z?" This requires either scraping public university pages (check terms of service) or a formal data-sharing partnership — which, given we are students doing non-profit work, this is actually a great case for direct outreach to university administrations. This is far more sustainable than any workaround.

### 1.2.3. Problems

**We'd be creating a honeypot**
Any database mapping names to institutional roles is attractive to scrapers, stalkers, or bad actors doing targeted harassment or social engineering ("I confirmed she's a professor there, now I know her department and can impersonate a colleague"). Minimize data retention — verify-on-request rather than building a stored, searchable directory, if we can.

**Consent and data protection**
Even "public" affiliation info can be sensitive depending on context (e.g., someone doesn't want their university affiliation publicly searchable for safety reasons). Algeria has data protection law (Law 18-07) — We'll have to understand what obligations apply if we're processing personal data, even informally.

## 1.3. Verifiable credentials / cryptographic approach (most robust, most work)

Universities issue signed digital credentials (like a signed JWT or a verifiable credential per W3C standards) that a person can present, and we verify the signature against the university's public key. This is the "correct" long-term architecture (no central database of PII for us to protect) but requires university buy-in to issue credentials, so it's a bigger lift.

## 1.4. Liability for false confirmations

If our tool wrongly confirms or denies someone's status, and someone relies on that for a decision (e.g., an editor deciding whether to trust a source), that's a real failure mode. Framing matters: "we could not confirm this affiliation" is safer than "this person is NOT a professor."

A narrower version of this would be something like a "credential-claim checker" that only handles the misinformation use case (verifying claimed expert credentials cited in viral posts) rather than a general people-lookup service. That would keep the privacy surface much smaller while still being useful. We could choose to go in that direction, or keeping this as a genuinely separate, more general product.

Or, we could focus on creating the solution for the institutions to implement cryptographic identification if the legal hassle of having a database is too much.

# 2. Misinformation

**A note on scope and risk**
- "Automated truth detection" is genuinely hard and can backfire (false positives erode trust, and heavy-handed labeling can look like censorship, which is politically sensitive in our context). Framing our tools as *aiding transparency and verification* rather than *arbitrating truth* will serve us better both technically and reputationally.
- Partnering with existing fact-checking orgs (there are regional ones, e.g., in the MENA fact-checking network) for labeled data and validation would save enormous time versus building from scratch.

## 2.1. Language and dialect tooling

Most misinformation detection tools are built for English or Modern Standard Arabic, but a lot of what circulates in Algeria is in Algerian Arabic (Darija), French, or code-switched mixes of the two, sometimes written in Arabizi (Latin script). This is a genuine gap we are well positioned to address:
- Build or expand an annotated dataset of Algerian misinformation/rumor examples in Darija and French — this alone would be valuable to the research community and is very doable as a student project (data collection, labeling guidelines, inter-annotator agreement).
- Fine-tune existing multilingual/Arabic-dialect models (e.g., AraBERT, CAMeLBERT, or open LLMs) on that dataset for claim detection or stance classification.

## 2.2. Detection and classification systems

- A claim-detection classifier that flags sentences likely to be checkable factual claims (separate from the harder task of judging truth/falsity).
- Source credibility scoring based on domain history, past fact-checks, and network signals (who shares it, how fast it spreads).
- Image/video tooling: reverse-image-search integration, basic manipulation/deepfake detection, or simply a bot that checks if a viral image has appeared before in a different context (a huge share of misinformation is "real photo, wrong caption").

## 2.3. Informatino Verification infrastructure

- A WhatsApp or Telegram bot where people forward a suspicious message/image and get back a rapid credibility check; tip-line-style bots have worked well elsewhere (e.g., in India, Latin America).
- A browser extension that flags known-false claims or shows credibility context on articles/posts as people browse.
- A shared database/API of fact-checks (ours plus international ones from AFP, Reuters, etc.) that other apps or researchers can query.

## 2.4. Human-in-the-loop / media literacy tools

- Since fully automated truth detection is unreliable and can misfire dangerously, many effective systems are decision-support tools for human fact-checkers rather than autonomous judges — e.g., a dashboard that surfaces trending claims, clusters similar posts, and prioritizes what fact-checkers should look at first.
- Interactive games or web tools that teach media literacy (several European "prebunking" games like Bad News have shown real effect — we could localize the concept with Algeria-relevant scenarios).
