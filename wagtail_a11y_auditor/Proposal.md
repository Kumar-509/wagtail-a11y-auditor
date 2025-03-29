Project Proposal: Wagtail Accessibility Auditor
Google Summer of Code 2025 Submission

Submitted by: KUMAR.K

Table of Contents
Abstract
1.1 Current Accessibility Challenges in Wagtail
1.2 Goals
1.3 Benefits
The Accessibility Auditor Framework
2.1 Overview
2.2 Advantages
Integration with Wagtail Admin
Schedule and Milestones
4.1 Core Framework Development
4.2 Admin Integration
4.3 Finalization and Polish
About Me
1. Abstract
1.1 Current Accessibility Challenges in Wagtail
Wagtail excels as a flexible CMS, but its accessibility tools are limited. Content editors lack real-time feedback on WCAG 2.1 compliance (e.g., missing alt text, poor color contrast), relying on external tools like WAVE or manual audits. Developers can’t easily enforce accessibility rules across page templates or third-party apps, and there’s no built-in mechanism to flag issues during content creation. This gap risks non-compliant sites, especially for public sector or inclusive projects, where accessibility is legally or ethically mandated.

1.2 Goals
Build a Third-Party Library: Create wagtail-a11y-auditor, a modular framework to audit and enforce accessibility in Wagtail pages and snippets.
Real-Time Feedback: Integrate live accessibility checks into the Wagtail admin interface.
Extensibility: Allow developers to define custom rules (e.g., “all images need alt text”) and extend audits to new content types.
Education: Provide actionable suggestions and WCAG references to guide editors.
Compatibility: Support Wagtail 6.x without disrupting existing workflows.
1.3 Benefits
User Empowerment: Editors gain tools to create accessible content without external dependencies.
Community Value: A reusable library strengthens Wagtail’s appeal for inclusive projects.
Reputation: Positions Wagtail as a leader in accessible CMS design.
Maintainability: Modular design and tests ensure long-term adoption.
2. The Accessibility Auditor Framework
2.1 Overview
wagtail-a11y-auditor will be a standalone Wagtail package that audits content for accessibility issues, yielding detailed reports. It uses a rule-based system inspired by linting tools, with built-in WCAG checks and a plugin architecture for custom rules.

Core Components:

Audit Rules: Callable classes defining accessibility checks (e.g., ImageAltTextRule).
Report Objects: Issue (severity, message, explanation, hint, wcag_ref).
Auditor Engine: Scans page/snippet content, applies rules, and aggregates results.
Example Rule:

python

Collapse

Wrap

Copy
from wagtail_a11y_auditor import Issue, AuditorRule

class ImageAltTextRule(AuditorRule):
    def check(self, page):
        for block in page.body:
            if block.block_type == "image" and not block.value.get("alt"):
                yield Issue(
                    severity="error",
                    message="Image missing alt text",
                    explanation="Alt text is required for screen readers (WCAG 2.1, 1.1.1).",
                    hint="Add descriptive alt text in the image block settings.",
                    wcag_ref="https://www.w3.org/WAI/WCAG21/quickref/#non-text-content"
                )
Supported Checks (Initial Set):

Missing alt text on images.
Insufficient color contrast in rich text (parsed via CSS analysis).
Unlabeled form fields in StreamField forms.
Heading hierarchy violations (e.g., skipping H2 to H4).
Technical Design:

Backend: Python/Django, leveraging Wagtail’s StructBlock and StreamField APIs.
Frontend: React-based UI hooks into Wagtail’s admin (via wagtail_hooks).
Storage: Caches audit results in Django’s cache framework for performance.
2.2 Advantages
Flexibility: Developers can add rules via subclassing, supporting custom blocks or apps.
Proactivity: Catches issues during editing, not post-publishing.
Scalability: Works with any Wagtail content type (pages, snippets, settings).
Education Focus: Links to WCAG docs help users learn accessibility best practices.
3. Integration with Wagtail Admin
The auditor will enhance the Wagtail admin with:

Live Feedback Panel: A sidebar widget showing issues as editors work (e.g., “2 errors, 1 warning”).
Fix Suggestions: Clickable hints (e.g., “Add alt text here”) linking to the relevant field.
Audit Command: wagtail a11y_audit to scan all pages/snippets programmatically.
Dashboard Summary: A card on the admin homepage flagging site-wide compliance (e.g., “90% of pages accessible”).
Example Workflow:

Editor adds an image block without alt text.
Sidebar updates: “Error: Image missing alt text” with a “Fix Now” button.
Editor clicks, enters alt text, and the issue clears instantly.
4. Schedule and Milestones
Duration: 12 weeks (May 26 – August 18, 2025, ~350 hours).

Size: Medium difficulty, ~350 hours.

Pre-Coding: Research WCAG rules, finalize API (April-May 2025).

4.1 Core Framework Development (6 weeks, May 26 – July 6)
Week 1-2 (40h): Setup & Rule Engine
Initialize wagtail-a11y-auditor repo, integrate with Wagtail 6.x.
Build AuditorRule base class, Issue model, and engine.
Milestone: Basic rule execution working.
Week 3-4 (60h): Initial Rules & Tests
Implement core rules (alt text, contrast, headings).
Write unit tests with mock pages/snippets.
Milestone: 3+ rules fully functional and tested.
Week 5-6 (50h): CLI & Docs
Add wagtail a11y_audit command.
Draft initial docs (setup, rule creation).
Milestone: CLI audits site, alpha release on PyPI.
4.2 Admin Integration (4 weeks, July 7 – August 3)
Week 7-8 (60h): Sidebar Widget
Build React-based live feedback panel using Wagtail’s UI hooks.
Connect to auditor engine via AJAX.
Milestone: Real-time feedback in admin.
Week 9-10 (50h): Dashboard & Enhancements
Add homepage summary card.
Refine UI with mentor feedback, add fix links.
Milestone: Full admin integration complete.
4.3 Finalization and Polish (2 weeks, August 4 – August 18)
Week 11 (30h): Testing & Refinement
Comprehensive integration tests across Wagtail versions.
Optimize performance (e.g., caching).
Milestone: Beta release, stable functionality.
Week 12 (20h): Docs & Release
Finalize docs (tutorials, API reference).
Publish v1.0.0 on PyPI, submit to GSoC.
Milestone: Project complete, demo-ready.
5. About Me
I’m K. Kumar, a 21yr old developer from Hyderabad, Telangana, India, with a passion for accessible web design and open-source. I’m a Computer Science undergrad at CMRTC (Pursuing 3rd year) and have been coding in Python for 2years.

Experience
Portfolio Site (2024): Built a Wagtail site with custom StreamFields, deployed on Render.
Accessibility Tool (2023): Created a Python script to audit HTML for WCAG compliance, using BeautifulSoup.
Open-Source: Contributed a bug fix to a Django app (e.g., GitHub PR #123, 2024).
Skills: Python, Django, Wagtail, JavaScript (React), HTML/CSS, Git, pytest.
Why I’m Up to the Task
My experience with Wagtail and accessibility auditing gives me a strong foundation. I’ve tackled medium-sized projects solo (e.g., my portfolio took ~200 hours), and my proactive debugging skills ensure I can handle challenges. I’m comfortable with Wagtail’s internals (hooks, admin UI) and have studied WCAG 2.1 extensively, making this a perfect fit. I thrive under mentorship and will deliver a polished, impactful library.

Motivation
I want to make the web more inclusive, and Wagtail’s community-driven ethos inspires me. GSoC is a chance to grow my skills, collaborate with experts, and contribute something meaningful to a tool I love. Accessibility matters deeply relies on screen readers, fueling my drive for this project.

Contact
Email: kumarcse42@gmail.com
GitHub: Kumar-509
IRC: Kumar (#wagtail, #gsoc)
Why Wagtail Should Accept This
Relevance: Accessibility is a growing priority (e.g., EU Web Accessibility Directive), and Wagtail needs native tools to stay competitive.
Impact: Empowers editors and developers, reducing reliance on external audits.
Feasibility: Scoped for 350 hours, leveraging existing Wagtail APIs, with clear milestones.
Innovation: Combines real-time feedback with extensibility, a unique addition to Wagtail’s ecosystem.
I’d welcome feedback from Wagtail admins to refine this further. Let’s make Wagtail the go-to CMS for accessibility!