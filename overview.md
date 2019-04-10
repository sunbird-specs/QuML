Assessment has always played an important role in education. Most, if not all, types of
formal education use some sort of assessment, typically including a final exam to earn
a grade, a degree, a license, or some other form of qualification.

Today, assessment is no longer restricted to grading at the end of an instruction
(summative assessment), but it has been recognized that assessment is also useful for
continuous monitoring & feedback (formative assessment) and guiding of the learning
progress (means to learn), without being necessarily used for grading purposes.

## Questions for continuous feedback

Formative assessment, including self-assessment, can play a vital role in motivating
students since it provides them with a way to judge their own competency level and
allows them to track their progress. It also enables students to identify areas where
more work is required, and to thereby remain motivated to improve further. Of course,
this requires that students receive feedback as quickly as possible.

Formative assessment also provides timely feedback for teachers, both with respect to
the effectiveness of the teaching and the performance of the students; it thus helps to
identify points that might need clarification.

For both groups, teachers and students, frequent testing is preferable. Infrequent
testing makes each exam a “major event”, with students investing much effort into
preparation and they may even stop attending class to prepare for the exam. With
infrequent tests, students may be unable to determine whether they are studying the
right material and with sufficient depth.

> ### Though it may be more time consuming for teachers, frequent testing reduces the importance of each individual exam and helps students to better gauge their progress.

Questions from a question bank can be used to create different types of formative
assessments. Presenting questions from a question bank has many advantages. Firstly,
it can ensure that questions are always fresh, do not become stale and repetitive and
the questions can evolve in their precision of measuring the student’s proficiency.

```
Figure: Summative and Formative Assessments
```

## Questions as a means to learn

Answering questions and solving problems is an effective way to learn. Unlike
traditional tests where the questions are used to measure the proficiency of a student,
if questions are presented as learning tools, they will encourage students to do a full
and meaningful enquiry about the related concepts. In fact, one of the main objectives
of questions should be “achieving defined goals”. A student should be asked questions
that will require him or her to use the skills that he or she is trying to learn. Such
questions should be more about provoking a process of learning than about finding an
answer.

If the questions focus on micro-concept level assessment of student’s proficiency, it is
possible to identify strengths, areas that need focus & improvement, and recommend
the relevant content to the student that specifically address the individual learning
needs.

Questions that are tagged with appropriate pedagogic metadata and associated with
relevant concepts will enable “questioning” to be used as an effective means to
encourage learning.

```
Figure: Questions in a question bank with proper tagging and associations will enable multiple use-cases
of questions
```

## Need for a common standard

Assessment is always a time-consuming activity for teachers, especially if large
numbers of students are to be assessed or, if assessment is frequent. This has motivated
the development of technical devices to support assessment, starting with relatively
simple mechanical devices and evolving to today’s ​computer-aided assessment (CAA) ​or
e-assessment​.

E-assessment is one of the fundamental elements of e-learning. E-assessments have a
number of practical advantages; in particular, scoring can be automated (to most
extent). This makes them especially attractive in e-learning settings, as it allows to
make assessment available “anyplace, anytime”.

Creating high-quality e-assessments, however, is challenging, especially if they are to
assess higher-order cognitive levels, such as application, analysis, synthesis, and
evaluation in the traditional taxonomy of Bloom.

> ### While e-assessments are indeed attractive, they are extremely expensive to construct: question creation is a highly refined and time-consuming art, especially if one expects to develop good questions that are relatively unambiguous.

Ensuring the reusability, longevity, and platform independence of tests can mitigate
the high costs of creation and can help preserve investments and intellectual assets
when hardware and software change, thus ensuring sustainability. This requires a
standard, platform-neutral, vendor-independent interchange file format for
e-assessments.

```
Figure: A common standard will enable distributed assessments authoring and delivery
```

The common standard should enable the following capabilities for e-learning systems:

#### ➢ Provide a standard content format for storing and exchanging questions independent of the authoring tool used to create them.

#### ➢ Support the deployment of question banks across a wide range of learning and assessment delivery systems.

#### ➢ Provide a standard content format for storing and exchanging tests independent of the test construction tool used to create them.

#### ➢ Support the deployment of questions, question banks and tests from diverse sources in a single learning or assessment delivery system.

#### ➢ Provide systems with the ability to report test results in a consistent manner.

A number of standards aiming to promote interoperability and sustainability of
e-learning content and e-assessment have emerged over the last couple of decades. ​IMS
Question and Test Interoperability ​(IMS QTI) is the best-known standard for tests.
However, IMS QTI is not used as the standard here primarily because of the following
two reasons:

1. QTI is a very large specification with many optional parts. It is very difficult to
ensure interoperability between systems that do not implement all parts of the
standard.
2. Although we see familiar elements like \<p> and \<ul> in the specification,
everything is in the QTI namespace and not in the XHTML namespace. The standard does not prescribe that you
have to render it with a browser, which leaves a lot of room for interpretation.
So, consistent rendering is a major problem.

This document specifies a model (for questions and tests) which addresses the above
concerns with ​IMS QTI specification. The proposed model is derived from the IMS QTI
specification, with changes made to meet the following requirements.

### Formal Definition

A standard or a specification intended as such should rely as far as possible on existing
and proven standards. This allows keeping the specification of the interchange format
concise and enables implementers to make use of available and proven tools. The
proposed model is primarily based on HTML and Javascript.

> ### HTML and javascript, being well-defined and globally acknowledged standards, makes it simple for implementations to interpret and transfer questions and tests in the form intended by the original author.

### Composability

There are different scenarios for the interchange of tests. In some cases, tests may be
reused in their entirety, whereas in other cases only individual questions from a test or
a test collection (​question bank​) may be integrated into another test. It is therefore
essential to clearly separate the different aspects of tests, especially ​individual questions
and ​sets ​in a test.

### Size and Scope

A model that is able to describe all of the functionality of all systems may seem
desirable to enable the interchange of complete tests with all their properties. Looking
closer, one can see that this requirement is illusionary: The facilities of test systems are
too diverse and too varied and no system supports all test and scoring types.

A model should therefore restrict itself to a relatively small “core set” of test and
question types. Further types can be added later on once it has become clear which
types are actually required in practice.

### Longevity

Finally, a specification should ensure longevity, i.e., questions and tests described
using the format should remain processable for as long as possible. For correcting
errors and extending the format, new revisions will become necessary from time to
time, but it must be avoided that new revisions interfere with the data interchange.
This means that different revisions should be compatible with each other as far as possible. Gratuitous incompatibilities must be avoided at all costs; sometimes,
however, incompatible changes may be necessary.

To mitigate the potential negative impact of such changes, rigorous revision
management is essential, starting with the distinction between major and minor
revisions and corresponding numbering schemes. Incompatible changes must then only
be introduced in major revisions, after having been announced before. Inside a major
revision, say,  1 .x, all minor revisions (e.g.,  1. 1 ,  1. 2 , etc.) are all compatible with each
other. Revision  2. 0  may introduce incompatible changes, but not  2. 1. This ensures that
implementers and users can easily and reliably decide whether a specific question or
test can be processed or not.

## QuML Specification

Question Markup Language (​QuML in short) is a specification for storage, rendering and
distribution of Questions and Tests. QuML allows assessment materials to be authored
and delivered on multiple systems interchangeably. It is designed to facilitate
interoperability between systems.

> ### QuML defines a standard format for representation of questions, tests and their results, supporting the exchange of this material between authoring and delivery systems, repositories and other e-learning systems.

This specification enables aggregating questions from a ​wide ​variety of existing
sources (Question papers, PDFs, documents, teachers, individuals, etc) into ​one
repository and stored in a ​common format​. With all questions available in a
repository and in the same format, the reach and applicability of these questions gets
amplified multi-fold. When these questions are available with QuML as the interface
language, multiple applications (which understand & render QuML) can get created to
serve a ​wide variety of use cases and users​.

In a nutshell, a microservices architecture centered around “questions”, with different
platforms & applications as constituent services, is possible. It allows anyone to offer a
scalable, entirely new application that uses QuML questions, effortlessly.

```
Figure: Services centered around ​“QuML Questions”
```

A lot of questions currently exist in PDFs, word documents, as exam papers (soft and
hard copies), in the minds of teachers & other creators, in existing question
repositories and in many other sources. And on the demand side, teachers, students &
parents have access to some of those resources only - mainly because existing systems
have access to questions from at most one source. And some of the questions are
completely not accessible as there are no systems or services built for them.

> ### QuML powered question repositories create more connections between existing supply of questions and the demand for learning (via questions).

If questions from multiple sources are imported into a QuML compliant question
repository, multiple systems get access to all the questions. It is possible because
systems need to adhere to only one specification (QuML), unlike before, where each
question is stored and represented in a different way.

