# Clean Code

<details> <summary> Table of Contents </summary>

- [Chapter 2 Meaningful Names](#meaningful-names)
    1. [USE INTENTION REVEALING NAMES](#use-intention-revealing-names)
    2. [AVOID DISINFORMATION](#avoid-disinformation)
    3. [MAKE MEANINGFUL DISTINCTIONS](#make-meaningful-distinctions)
    4. [USE PRONOUNCEABLE NAMES](#use-pronounceable-names)
    5. [USE SEARCHABLE NAMES](#use-searchable-names)
    6. [AVOID ENCODINGS](#avoid-encodings)
    7. [AVOID MENTAL MAPPING](#avoid-mental-mapping)
    8. [CLASS NAMES](#class-names)
    9. [METHOD NAMES](#method-names)
    10. [DON’T BE CUTE](#don't-be-cute)
    11. [PICK ONE WORD PER CONCEPT](#one-word-per-concept)
    12. [DON'T PUN](#don't-pun)
    13. [USE SOLUTION DOMAIN NAMES](#solution-domain)
    14. [USE PROBLEM DOMAIN NAMES](#problem-domain)
    15. [ADD MEANINGFUL CONTEXT](#meaningful-context)
    16. [DON’T ADD GRATUITOUS CONTEXT](#gratuitous-context)
- [Chapter 3 Functions](#functions)

    1. [REFACTOR](#refactor)
    2. [SMALL](#small)
    3. 

</details>

## Meaningful Names
1.  USE INTENTION REVEALING NAMES <a name="use-intention-revealing-names"/>  

    ```java
    int elapsedTimeInDays;
    int daysSinceCreation;
    int daysSinceModification;
    int fileAgeInDays;
    ```
    - Case Study
        ```java
            //Orignal Code
            public List<int[]> getThem() {
            List<int[]> list1 = new ArrayList<int[]>();
            for (int[] x : theList)
            if (x[0] == 4)
                list1.add(x);
            return list1;
        }
        ```
        - Questions we should ask ourselves
            1. What kinds of things are in theList?

            2. What is the significance of the zeroth subscript of an item in theList?

            3. What is the significance of the value 4?

            4. How would I use the list being returned?

        ```java
        //modified code
        public List<Cell> getFlaggedCells() {
        List<Cell> flaggedCells = new ArrayList<Cell>();
        for (Cell cell : gameBoard)
        if (cell.isFlagged())
            flaggedCells.add(cell);
        return flaggedCells;
    }
    ```

2. AVOID DISINFORMATION <a name="avoid-disinformation"/>  
    - Example: Do not refer to a grouping of accounts as an accountList unless it’s actually a List. The word list means something specific to programmers. If the container holding the accounts is not actually a List, it may lead to false conclusions.1 So accountGroup or bunchOfAccounts or just plain accounts would be better.
    - Beware of using names which vary in small ways. How long does it take to spot the subtle difference between a `XYZControllerForEfficientHandlingOfStrings` in one module and, somewhere a little more distant, `XYZControllerForEfficientStorageOfStrings`? The words have frightfully similar shapes.
    - **Spelling similar concepts similarly is information. Using inconsistent spellings is disinformation.**
    - A truly awful example of disinformative names would be the use of lower-case L or uppercase O as variable names, especially in combination.
        ```java
        int a = l;
        if ( O == l )
            a = O1;
        else
            l = 01;
        ```
3. MAKE MEANINGFUL DISTINCTIONS<a name="make-meaningful-distinctions"/>  
    - It is not sufficient to add number series or noise words, even though the compiler is satisfied. If names must be different, then they should also mean something different.
    - How are the programmers in this project supposed to know which of these functions to call?
        ```java
        getActiveAccount();
        getActiveAccounts();
        getActiveAccountInfo();
        ```

    - In the absence of specific conventions, the variable `moneyAmount` is indistinguishable from `money`, `customerInfo` is indistinguishable from `customer`, `accountData` is indistinguishable from `account`, and `theMessage` is indistinguishable from `message`. Distinguish names in such a way that the reader knows what the differences offer.

4. USE PRONOUNCEABLE NAMES<a name="use-pronounceable-names"/>  

    From

    ```java
    class DtaRcrd102 {
     private Date genymdhms;
     private Date modymdhms;
     private final String pszqint = ”102”;
      /* … */
   };
   ```

   to 

   ```java
   class Customer {
     private Date generationTimestamp;
     private Date modificationTimestamp;;
     private final String recordId = ”102”;
     /* … */
   };
   ```

5. USE SEARCHABLE NAMES<a name="use-searchable-names"/>  

    From
    ```java
    for (int j=0; j<34; j++) {
     s += (t[j]*4)/5;
   }
   ```

   to
    ```java
    int realDaysPerIdealDay = 4;
    const int WORK_DAYS_PER_WEEK = 5;
    int sum = 0;
    for (int j=0; j < NUMBER_OF_TASKS; j++) {
        int realTaskDays = taskEstimate[j] * realDaysPerIdealDay;
        int realTaskWeeks = (realdays / WORK_DAYS_PER_WEEK);
        sum += realTaskWeeks;
    }
   ```
6. AVOID ENCODINGS<a name="avoid-encodings"/>  
    - Hungarian Notation was good in the old days because old compiler did not check types in those days. Not needed as today's comprogramming languages such as java don't need type encoding.
    - Member Prefixes
        - You also don’t need to prefix member variables with m_ anymore.
    - Interfaces and Implementations
        - Calling it ShapeFactoryImp to distinguish with ShapeFactory (interface)
7. AVOID MENTAL MAPPING<a name="avoid-mental-mapping"/>  
    - This is a problem with single-letter variable names.
    - One difference between a smart programmer and a professional programmer is that the professional understands that clarity is king. Professionals use their powers for good and write code that others can understand.
8. CLASS NAMES<a name="class-names"/>  
    - Classes and objects should have noun or noun phrase names like Customer, WikiPage, Account, and AddressParser. Avoid words like Manager, Processor, Data, or Info in the name of a class. A class name should not be a verb.
9. METHOD NAMES<a name="method-names"/>  
    - Methods should have verb or verb phrase names like `postPayment`, `deletePage`, or `save`. `Accessors`, `mutators`, and `predicates` should be named for their value and prefixed with `get`, `set`, and `is` according to the javabean standard
    - `Complex fulcrumPoint = Complex.FromRealNumber(23.0);` is generally better than `Complex fulcrumPoint = new Complex(23.0);`
10. DON’T BE CUTE<a name="don't-be-cute"/>  
    - Cuteness in code often appears in the form of colloquialisms or slang. For example, don’t use the name `whack()` to mean `kill()`. Don’t tell little culture-dependent jokes like `eatMyShorts()` to mean `abort()`.
11. PICK ONE WORD PER CONCEPT<a name="one-word-per-concept"/>
    - Pick one word for one abstract concept and stick with it. For instance, it’s confusing to have `fetch`, `retrieve`, and `get` as equivalent methods of different classes. 
    - Likewise, it’s confusing to have a `controller` and a `manager` and a `driver` in the same code base. What is the essential difference between a `DeviceManager` and a `Protocol-Controller`? Why are both not `controllers` or both not `managers`? Are they both `Drivers` really? The name leads you to expect two objects that have very different type as well as having different classes.
12. DON’T PUN<a name="don't-pun"/>  
    - Avoid using the same word for two purposes. Using the same term for two different ideas is essentially a pun.
    - e.g., one might decide to use the word add for “consistency” when he or she is not in fact adding in the same sense. Let’s say we have many classes where add will create a new value by adding or concatenating two existing values. Now let’s say we are writing a new class that has a method that puts its single parameter into a collection. Should we call this method `add`? It might seem consistent because we have so many other add methods, but in this case, the semantics are different, so we should use a name like `insert` or `append` instead. To call the new method add would be a pun.
    
13. USE SOLUTION DOMAIN NAMES<a name="solution-domain"/>
    - The name `AccountVisitor` means a great deal to a programmer who is familiar with the `VISITOR` pattern. What programmer would not know what a `JobQueue` was? There are lots of very technical things that programmers have to do. Choosing **technical names** for those things is usually the most appropriate course.

14. USE PROBLEM DOMAIN NAMES<a name="problem-domain"/>
    - When there is no “programmer-eese” for what you’re doing, use the name from the problem domain. At least the programmer who maintains your code can ask a domain expert what it means.
    - Separating solution and problem domain concepts is part of the job of a good programmer and designer. The code that has more to do with problem domain concepts should have names drawn from the problem domain.
15. ADD MEANINGFUL CONTEXT<a name="meaningful-context"/>
    - Imagine that you have variables named `firstName`, `lastName`, `street`, `houseNumber`, `city`, `state`, and `zipcode`. Taken together it’s pretty clear that they form an address. But what if you just saw the state variable being used alone in a method? Would you automatically infer that it was part of an address?
    - You can add context by using prefixes: `addrFirstName`, `addrLastName`, `addrState`, and so on. At least readers will understand that these variables are part of a larger structure. Of course, a better solution is to create a class named Address. Then, even the compiler knows that the variables belong to a bigger concept.
    - Variables with unclear context
        ```java
        private void printGuessStatistics(char candidate, int count) {
            String number;
            String verb;
            String pluralModifier;
            if (count == 0) {
                number = ”no”;
                verb = ”are”;
                pluralModifier = ”s”;
            } else if (count == 1) {
                number = ”1”;
                verb = ”is”;
                pluralModifier = ””;
            } else {
                number = Integer.toString(count);
                verb = ”are”;
                pluralModifier = ”s”;
            }
            String guessMessage = String.format(
                ”There %s %s %s%s”, verb, number, candidate, pluralModifier
            );
            print(guessMessage);
        }
        ```
    - Variables have a context
        ```java
        public class GuessStatisticsMessage {
            private String number;
            private String verb;
            private String pluralModifier;
        
            public String make(char candidate, int count) {
            createPluralDependentMessageParts(count);
                return String.format(
                "There %s %s %s%s", 
                verb, number, candidate, pluralModifier );
            }
        
            private void createPluralDependentMessageParts(int count) {
            if (count == 0) {
                thereAreNoLetters();
            } else if (count == 1) {
                thereIsOneLetter();
            } else {
                thereAreManyLetters(count);
            }
            }
        
            private void thereAreManyLetters(int count) {
            number = Integer.toString(count);
            verb = "are";
            pluralModifier = "s";
            }
        
            private void thereIsOneLetter() {
            number = "1";
            verb = "is";
            pluralModifier = "";
            }
        
            private void thereAreNoLetters() {
            number = "no";
            verb = "are";
            pluralModifier = "s";
            }
        }
        ```
16. DON’T ADD GRATUITOUS CONTEXT<a name="gratuitous-context"/>
    - In an imaginary application called “Gas Station Deluxe,” it is a bad idea to prefix every class with `GSD`. Frankly, you are working against your tools. You type G and press the completion key and are rewarded with a mile-long list of every class in the system. Is that wise? Why make it hard for the IDE to help you?
    - The names `accountAddress` and `customerAddress` are fine names for **instances of the class** `Address` but could be poor names for **classes**. `Address` is a fine name for **a class**. If I need to differentiate between MAC addresses, port addresses, and Web addresses, I might consider `PostalAddress`, `MAC`, and `URI`. The resulting names are more precise, which is the point of all naming.

## Functions

1. Refactor
    - HtmlUtil.java 2007-06-19
    ```java
     public static String testableHtml(
     PageData pageData,
     boolean includeSuiteSetup
    ) throws Exception {
        WikiPage wikiPage = pageData.getWikiPage();
        StringBuffer buffer = new StringBuffer();
        if (pageData.hasAttribute("Test")) {
        if (includeSuiteSetup) {
            WikiPage suiteSetup =
            PageCrawlerImpl.getInheritedPage(
                    SuiteResponder.SUITE_SETUP_NAME, wikiPage
            );
            if (suiteSetup != null) {
            WikiPagePath pagePath =
                suiteSetup.getPageCrawler().getFullPath(suiteSetup);
            String pagePathName = PathParser.render(pagePath);
            buffer.append("!include -setup .")
                    .append(pagePathName)
                    .append("\n");
            }
        }
        WikiPage setup = 
            PageCrawlerImpl.getInheritedPage("SetUp", wikiPage);
        if (setup != null) {
            WikiPagePath setupPath =
            wikiPage.getPageCrawler().getFullPath(setup);
            String setupPathName = PathParser.render(setupPath);
            buffer.append("!include -setup .")
                .append(setupPathName)
                .append("\n");
        }
        }
        buffer.append(pageData.getContent());
        if (pageData.hasAttribute("Test")) {
        WikiPage teardown = 
            PageCrawlerImpl.getInheritedPage("TearDown", wikiPage);
        if (teardown != null) {
            WikiPagePath tearDownPath =
            wikiPage.getPageCrawler().getFullPath(teardown);
            String tearDownPathName = PathParser.render(tearDownPath);
            buffer.append("\n")
                .append("!include -teardown .")
                .append(tearDownPathName)
                .append("\n");
        }
            if (includeSuiteSetup) {
            WikiPage suiteTeardown =
                PageCrawlerImpl.getInheritedPage(
                        SuiteResponder.SUITE_TEARDOWN_NAME,
                        wikiPage
                );
            if (suiteTeardown != null) {
                WikiPagePath pagePath =
                suiteTeardown.getPageCrawler().getFullPath (suiteTeardown);
                String pagePathName = PathParser.render(pagePath);
                buffer.append("!include -teardown .")
                    .append(pagePathName)
                    .append("\n");
            }
            }
        }
        pageData.setContent(buffer.toString());
        return pageData.getHtml();
    }
    ```
    - Refactored HtmlUtil.java
        ```java
        public static String renderPageWithSetupsAndTeardowns(
        PageData pageData, boolean isSuite
        ) throws Exception {
            boolean isTestPage = pageData.hasAttribute("Test");
            if (isTestPage) {
            WikiPage testPage = pageData.getWikiPage();
            StringBuffer newPageContent = new StringBuffer();
            includeSetupPages(testPage, newPageContent, isSuite);
            newPageContent.append(pageData.getContent());
            includeTeardownPages(testPage, newPageContent, isSuite);
            pageData.setContent(newPageContent.toString());
            }
            
            return pageData.getHtml();
        }
    ```
2. SMALL
    - re-refactored HtmlUtil.java
    ```java
        public static String renderPageWith SetupsAndTeardowns(
        PageData pageData, boolean isSuite) throws Exception {
            if (isTestPage(pageData))
            includeSetupAndTeardownPages(pageData, isSuite);
            return pageData.getHtml();
        }
    ```
3. Blocks and Indenting








