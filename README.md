
# A note on scope and risk
- "Automated truth detection" is genuinely hard and can backfire (false positives erode trust, and heavy-handed labeling can look like censorship, which is politically sensitive in our context). Framing our tools as *aiding transparency and verification* rather than *arbitrating truth* will serve us better both technically and reputationally.
- Partnering with existing fact-checking orgs (there are regional ones, e.g., in the MENA fact-checking network) for labeled data and validation would save enormous time versus building from scratch.

# Ideas

## Language and dialect tooling

Most misinformation detection tools are built for English or Modern Standard Arabic, but a lot of what circulates in Algeria is in Algerian Arabic (Darija), French, or code-switched mixes of the two, sometimes written in Arabizi (Latin script). This is a genuine gap we are well positioned to address:
- Build or expand an annotated dataset of Algerian misinformation/rumor examples in Darija and French — this alone would be valuable to the research community and is very doable as a student project (data collection, labeling guidelines, inter-annotator agreement).
- Fine-tune existing multilingual/Arabic-dialect models (e.g., AraBERT, CAMeLBERT, or open LLMs) on that dataset for claim detection or stance classification.

## Detection and classification systems
- A claim-detection classifier that flags sentences likely to be checkable factual claims (separate from the harder task of judging truth/falsity).
- Source credibility scoring based on domain history, past fact-checks, and network signals (who shares it, how fast it spreads).
- Image/video tooling: reverse-image-search integration, basic manipulation/deepfake detection, or simply a bot that checks if a viral image has appeared before in a different context (a huge share of misinformation is "real photo, wrong caption").

## Verification infrastructure
- A WhatsApp or Telegram bot where people forward a suspicious message/image and get back a rapid credibility check; tip-line-style bots have worked well elsewhere (e.g., in India, Latin America).
- A browser extension that flags known-false claims or shows credibility context on articles/posts as people browse.
- A shared database/API of fact-checks (ours plus international ones from AFP, Reuters, etc.) that other apps or researchers can query.

## Human-in-the-loop / media literacy tools
- Since fully automated truth detection is unreliable and can misfire dangerously, many effective systems are decision-support tools for human fact-checkers rather than autonomous judges — e.g., a dashboard that surfaces trending claims, clusters similar posts, and prioritizes what fact-checkers should look at first.
- Interactive games or web tools that teach media literacy (several European "prebunking" games like Bad News have shown real effect — we could localize the concept with Algeria-relevant scenarios).
