---
title: Status Page

---

# My First Attempt at Building with AI Agents

Atlassian Statuspage costs too much for what I actually need. I've explored alternatives like Instatus, but they came packed with features I'd never use... automatic monitoring integrations, complex workflows, and enterprise bells and whistles.

All I wanted was simple: to be able to manually update product statuses and craft incident messages when things go wrong. No frills, no automation, just a clean interface to communicate with users during outages.

So I decided to build one. But I thought I'd experiment with something new - GPT-4.1 in agent mode within VS Code. This was my first real attempt at using AI agents for a complete project, and I honestly wasn't sure what to expect.

## The Big Prompt

PHP was my language of choice, and I chose Symfony as my framework, because I know it well. I figured if I'm going to be reviewing AI-generated code, I should probably stick with something familiar.

I knocked together what I thought was a comprehensive initial prompt. A few paragraphs describing the entities I envisioned: `Product`, `Incident`, `IncidentMessage`, and `User`. I told it I wanted a page that looked like the default Atlassian Statuspage design and that I wanted an API to perform CRUD operations on those entities. I suggested it could deviate from my design if it had better ideas, and honestly, I was curious to see what it would come up with.

The response was immediate and overwhelming. GPT-4.1 generated what felt like hundreds of lines of code across multiple files... entities, controllers, templates, CSS, everything. Unfortunately, it didn't get creative at all; it implemented exactly the entities I'd suggested (thought it did choose to use ApiPlatform for the API layer). On the one hand, I was slightly disappointed it didn't surprise me with better ideas. On the other, everything it created actually worked on the first try, which was pretty cool.

## The Review Marathon

I spent the next chunk of time diving into the generated code, trying to understand what the AI had built. The structure was clean and followed Symfony conventions perfectly. The entities had proper relationships, the templates were well-organised, and the CSS was surprisingly decent. But reviewing this much code at once was more mentally taxing than I'd expected. I found myself jumping between files, tracing relationships, and trying to build a mental model of the entire application. Unlike being the actual developer of the application, where you know the full history of the application as it was gently crafted, this was like somebody emailing you the source of an entire finished app and asking you to review it for them.

Anyway, once I felt comfortable with the codebase, I wanted to test it properly. I asked the model to help me with that by inserting some sample data using the APIs it had created, thinking this would be straightforward.

This is where things got weird.

Despite having created perfectly functional entity relationships, the AI seemed completely confused about how to actually use them. It kept generating cURL commands that didn't make sense, mixing up foreign keys and failing to understand the basic flow of creating a `Product`, then an `Incident` for that `Product`, then `IncidentMessages` for that `Incident`.

After watching it struggle for a while, I gave up and inserted the test data myself. It only took me a few minutes once I understood the API structure, but the AI's confusion was puzzling. It had built something it couldn't seem to conceptually understand.

## Design Iterations and Surprising Successes

With real data in place, I could finally see the frontend in action. The basic layout was there, but it needed some polish. I asked for the colored status squares to be spaced evenly across each product row, and for the whole thing to be mobile responsive. The AI handled both requests smoothly, generating clean CSS that worked exactly as expected.

Then I got more ambitious.

I wanted users to be able to click on red or amber status squares to see the incidents for that day. I asked for the incidents and related messages to be loaded via Ajax and displayed below the products without requiring a page refresh. This felt like a complex request to me, but GPT-4.1 tackled it beautifully. It generated JavaScript that made API calls, processed the responses, and dynamically inserted HTML into the page. I only had to ask for one small tweak - changing the color of the incident container to match the status square that was clicked.

Feeling confident, I pushed for one more UX improvement: when a user clicks on a green square (indicating no incidents), it should close any currently open incident containers. This seemed like the simplest request yet, but the AI completely failed at it. Over and over, it generated code that didn't work, misunderstood the existing JavaScript, or created conflicting event handlers. Eventually, I got frustrated and fixed it myself by deleting just a couple of lines of code. The simplicity of the fix made the AI's repeated failures even more baffling.

## The Great Refactoring

Once the frontend behavior was working how I wanted, I decided to simplify the backend. ApiPlatform was overkill for my needs, and I wanted more control over the API responses. I asked GPT-4.1 to remove ApiPlatform entirely and replace it with a custom controller.

Instead of properly removing ApiPlatform, it just commented out all the related code, leaving a messy codebase full of dead comments. I had to go through and clean that up manually, which was tedious but not difficult.

However, once I specified exactly what I wanted for the replacement, the model redeemed itself completely. I gave it a detailed list of routes: "create products", "list products", "list incidents for a product", "create incidents", "update incidents", and "create incident messages". GPT-4.1 translated these specifications into a perfectly crafted ApiController with aptly named functions and clean Doctrine queries. Every route worked flawlessly on the first try.

## Building the Admin Interface

For the final piece, I wanted an admin backend that would let me interact with these new APIs through a web interface. I asked for an index page with links to forms for each operation, styled to match the frontend design language, with the links appearing as buttons containing relevant icons or emojis.

The model absolutely nailed this request. It created a clean admin interface with HTTP Basic authentication, intuitive navigation, and forms that perfectly matched the visual style of the public status page. The attention to detail was impressive... it even chose appropriate emojis for each function and organized everything logically.

## Four Hours

By the end of the session, I had a fully functional status page that did exactly what I needed. The whole process took about four hours of intermittent attention while I multitasked on other work. I'd periodically check back to review what the model had generated, or approve commands it needed permission to run.

The experience felt like working with a brilliant but unpredictable collaborator. Complex, well-defined tasks often succeeded, while simple logical operations sometimes failed repeatedly. The AI could generate sophisticated Ajax interactions but couldn't figure out how to hide a div. It could create perfect entity relationships, but couldn't understand how to use them for data insertion.

What struck me most was how the codebase evolved through this back-and-forth process. Each iteration built meaningfully on the previous version, and the AI generally maintained consistency with earlier architectural decisions. When I asked for changes, it usually preserved the existing structure while adding the new functionality cleanly.

## Looking Forward

The foundation is solid enough that I'm planning to open source the codebase and possibly offer a hosted version for users who don't want to deploy it themselves. I still want to migrate from MySQL to either PostgreSQL or Redis, to upgrade the authentication system, and to add features like Slack integration and incident notifications. But for my first experiment with AI agents, getting a working alternative to expensive SaaS tools in one afternoon feels like a genuine win.

Would I approach future projects this way? Probably, though I suspect each one will teach me new lessons about where AI excels and where it stumbles. The time savings were real, but so was the mental overhead of constant code review and the occasional need to step in and fix things manually.

Working with AI agents turned out to be less about giving up control and more about learning to collaborate with a very capable but quirky partner. The key seems to be understanding when to trust the AI's output and when to take the reins yourself.