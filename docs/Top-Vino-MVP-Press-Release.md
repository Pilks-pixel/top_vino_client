# TOP VINO helps wine students remember facts and practise explaining them

*Draft launch press release for the full MVP*

**A phone-first study app that pairs spaced-repetition flashcards with checklist-guided written practice**

Preparing for WSET Diploma theory exams means remembering a lot of detail and being able to explain how those details connect. A learner might remember that higher altitude can mean cooler growing conditions, then struggle to explain how those conditions can affect ripening and acidity. When revision time is short, it’s also hard to decide which material needs another pass.

TOP VINO starts with a private, invitation-only flashcard beta. Learners can copy an original starter deck or make their own cards from course notes and organize their decks by topic. Due cards come first. Learners try to recall the answer, reveal it, and rate how well they remembered it. TOP VINO uses that rating to schedule the next review and limits how many new cards are introduced each day. When learners return, they can see what they reviewed, what’s due, and what’s coming up.

The next milestone adds short written practice. Learners write an explanation, then compare it with a reviewed example answer and checklist. A prompt might ask how cooler growing conditions can affect a wine’s acidity. After writing, a learner can check whether they linked growing temperature, ripening, and acidity, then revise the part they missed.

TOP VINO is an independent study companion for learners who already have course materials and want a regular way to revise between classes or before an exam.

---

## Internal MVP scope

**Primary learner.** A self-directed student who already has notes or course materials and revises in short sessions, often on a phone. The beta’s starter material is for WSET Level 3 and Diploma theory learners.

**Product promise.** I can see what needs review, practise recalling it, and improve how I explain it. When I return, my progress is saved and I know where to start.

| Learner problem | Smallest MVP capability | What we should be able to observe |
| --- | --- | --- |
| “I have notes, but getting started takes work.” | One small, original starter deck on a single topic. Learners can copy it or make a private deck, then add and edit basic or fill-in-the-blank cards. | A learner copies the deck, adds a question from their notes, and completes a first review without help. |
| “I don’t know what to revise today.” | A due-first review queue, a daily cap on new cards, and saved scheduling based on self-rated recall. | After a review, the learner can return on another day, see which cards are due, and pick up without rebuilding a list. A retried submission records one review. |
| “I recognise the material, but my written answer misses the reasoning.” | A handful of original short-answer prompts on the same topic. Each has a reviewed example answer, a checklist, and a chance to revise after comparison. | The learner identifies an omitted point or weak link and improves their answer on a second attempt. |
| “I can’t tell what I’ve done or what’s coming.” | A progress view showing reviews completed today, cards due now, and scheduled reviews over the next seven days. | The learner can use the counts to decide whether to study now and plan their next session. |

**V1 and the full MVP.** V1 is the private flashcard loop, including account access and reset, private and recoverable study data, and a $0 operating budget. The full MVP is **recall → explain → revise**. The next milestone adds checklist-based written practice on the starter deck’s topic. Written practice runs separately from the flashcard schedule.

**Build filter.** Prioritize work that helps learners complete their first review, return to due cards, improve a written explanation, or keep their study data safe. Use the V1 specification for beta implementation details.

**MVP acceptance.** Ask learners in the 5–10 person beta to use the app between study sessions. Look for the same learners completing a first review, returning on a later day to review due cards, and pointing to one concrete improvement they made to a written answer. Use what interrupted that journey to decide what needs fixing before polishing the app.

### Deferred decisions

- **AI creation.** Whether to generate draft flashcards or practice prompts from learner-supplied materials.
- **AI feedback.** Whether to give targeted feedback on a written answer against its checklist.
- **Paid access.** Whether to charge for access and which features any paid access would cover.
