# Using PMD

Pick a Java project from Github (see the [instructions](../sujet.md) for suggestions). Run PMD on its source code using any ruleset. Describe below an issue found by PMD that you think should be solved (true positive) and include below the changes you would add to the source code. Describe below an issue found by PMD that is not worth solving (false positive). Explain why you would not solve this issue.

## Answer

 PreserveStackTrace
		Priority: Medium (3)
		True positive
		
		/home/guypi/Documents/commons-collections-master/src/main/java/org/apache/commons/collections4/CollectionUtils.java:1459:	
		PreserveStackTrace: New exception is thrown in catch block, original stack trace may be lost
		
		The PreserveStackTrace tries to detect when the stack trace is preserved when the exception is wrapped as an
		 exception, but fails when using a generator type template
		This rule is deprecated
		
		Throwing a new exception from a catch block without passing the original exception in a new exception will result in 
		the loss of the original stack trace, which will make debugging difficult indeed.
	
		This rule is defined by the following Java class: https://github.com/pmd/pmd/blob/master/pmd-java/src/main/java/net/sourceforge/
		pmd/lang/java/rule/bestpractices/PreserveStackTraceRule.java
		
		Should we solve it? This is a difficult question, as it requires a multi-class dataflow analysis to be able to do 
		be able to get it right.

		Our proposal:
			throw new IllegalArgumentException("Unsupported object type: " + object.getClass().getName());
		
		Certainly an improvement, but it's unlikely that we can tackle it
		
		
	- CompareObjectsWithEquals
		True/False positive
	
	

		In our case, we don't know what the developers want to do. If they want to compare references, so that the 
		rule does not apply. Then they are right to use ==.

		But most of the time it is a mistake of Java developers who try to compare the value of objects by using 
		== instead of .equals().

		
		
	- UncommentedEmptyConstructor
		False positive
		
		It finds places where the non-private constructor contains no statements or just contains super()and there is no 				
		comment inside. The previous Javadoc is not relevant for this rule.

		By providing comments in empty constructors, it is easier to distinguish between intentional 
		intentional (e.g. to be able to provide Javadoc) and unintentional (someone forgot to write the implementation or 
		this constructor can be simply deleted). 

Translated with www.DeepL.com/Translator (free version)
