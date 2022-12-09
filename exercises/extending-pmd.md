# Extending PMD

Use XPath to define a new rule for PMD to prevent complex code. The rule should detect the use of three or more nested `if` statements in Java programs so it can detect patterns like the following:

```Java
if (...) {
    ...
    if (...) {
        ...
        if (...) {
            ....
        }
    }

}
```
Notice that the nested `if`s may not be direct children of the outer `if`s. They may be written, for example, inside a `for` loop or any other statement.
Write below the XML definition of your rule.

You can find more information on extending PMD in the following link: https://pmd.github.io/latest/pmd_userdocs_extending_writing_rules_intro.html, as well as help for using `pmd-designer` [here](https://github.com/selabs-ur1/VV-TP2/blob/master/exercises/designer-help.md).

Use your rule with different projects and describe you findings below. See the [instructions](../sujet.md) for suggestions on the projects to use.

## Answer
In order to realize our new rule for Pmd we used pmd-designer And then wrote our rule with xpath  We had to look for an ifstatement which had two ancestors also ifstatement Once the rule was written we exported it to xml format We then added the ruleset tags We went to the pmd-bin-6.51.0/lib folder. Then we open pmd-Java-6.51.0. Then in rulesets/Java we add our xml file and we test our rule
   
    <ruleset>
        <rule name="nestedIf" language="java" message="use of three or more nested if statements" class="net.sourceforge.pmd.lang.rule.XPathRule">
            <description> </description>
            <priority>3</priority>
            <properties>
                <property name="version" value="2.0"/>
                <property name="xpath">
                <value>
                    /IfStatement[count(ancestor::IfStatement)>=2]
                </value>
                </property>
            </properties>
        </rule>
    </ruleset>
   
We will then apply it to one of the projects with the command:

    run.sh pmd -d /home/guypi/Documents/commons-collections-master -f text -R rulesets/java/nested.xml

Below is a preview of the result

    déc. 09, 2022 10:40:55 PM net.sourceforge.pmd.RuleSetFactory parseRuleSetNode
    AVERTISSEMENT: RuleSet name is missing. Future versions of PMD will require it.
    déc. 09, 2022 10:40:56 PM net.sourceforge.pmd.RuleSetFactory parseRuleSetNode
    AVERTISSEMENT: RuleSet description is missing. Future versions of PMD will require it.
    déc. 09, 2022 10:40:56 PM net.sourceforge.pmd.PMD encourageToUseIncrementalAnalysis
    AVERTISSEMENT: This analysis could be faster, please consider using Incremental Analysis: https://pmd.github.io/pmd-6.51.0/pmd_userdocs_incremental_analysis.html
    /home/guypi/Documents/commons-collections-master/src/main/java/org/apache/commons/collections4/CollectionUtils.java:1507:    nestedIf:    use of three or more       nested if statements
    /home/guypi/Documents/commons-collections-master/src/main/java/org/apache/commons/collections4/MapUtils.java:229:    nestedIf:    use of three or more nested if     statements
    /home/guypi/Documents/commons-collections-master/src/main/java/org/apache/commons/collections4/MapUtils.java:232:    nestedIf:    use of three or more nested if     statements
    /home/guypi/Documents/commons-collections-master/src/main/java/org/apache/commons/collections4/MapUtils.java:235:    nestedIf:    use of three or more nested if    statements
