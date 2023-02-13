# Analysejar description

### **Usage pattern**:
```
Java -jar analyseJar.jar -c callstack -j jar_to_analyse -option
```
The jar_to_analyse is the upstream jar that needs to be analyzed, and the anaylse_option specifies which feature(path length/constraint type/...) to get from jar_to_analyse.

### **Option usage**:
```
 -bd,--CVEBlockDepth                  get block depth of target CVE
 -c,--callStackFile <arg>             File thatcontains call stack
 -cl,--ConstraintLength               get the number of constraint along
                                      the path
 -co,--ConstraintOperand              get the operand of constraint along
                                      the path
 -cr,--ConstraintVariableReturnType   get information about return type of
                                      the constraint variable
 -cs,--CallSiteInfo                   get information about the call
                                      site:Guarded@Returned@AsPara
 -cv,--ConstraintVariable             get the type of variable involved in
                                      the constraint along the path
 -h,--help                            Print usage
 -j,--jarName <arg>                   Jar file that to be analysed
 -md,--MethodBlockDepth               get block depth of method in target
                                      CVE
 -pc,--PathCoverage                   get path coverage of the path
```

The detailed explanation of the option is as follows:

+ **CVEBlockDepth**. This option analyzes the CFG block numbers along the path to the vulnerable function of the certain CVE (which may involve multiple vulnerable functions) specified by the callstack file. It returns the average block numbers of the CVE.
+ **MethodBlockDepth**. This option also analyzes the CFG block numbers, but it differs from CVEBlockDepth that it will provide the average block numbers of each vulnerable function involved in the CVE.
+ **ConstraintLength** This option returns the number of constraints along the path.
+ **ConstraintVariable** This option returns the variable types involved in the constraint along the path.
+ **ConstraintOperand** This option returns the operands involved in the constraint along the path.
+ **PathCoverage** This option returns the path coverage along the path.
+ **CallSiteInfo** This option analyzes the call site information at the downstream, when using this option, users should use the RI files from /analyse jar/example/RI instead of callstack files.

### **Examples**

Example1:

This example takes the CVE-2016-9487@org.idpf:epubcheck:4.0.1 as the input upstream jar, and uses ConstraintOperand(co) as analyse option to get the operand type of the constraint alone the call path. The output is the operand type defined in soot, where soot.jimple.internal.JEqExpr refers to "==" operand, soot.jimple.internal.JNeExpr refers to "!=", and more operand type information can be found at: https://www.sable.mcgill.ca/soot/doc/soot/jimple/internal/AbstractJimpleIntBinopExpr.html

**<font color=green>command:</font>**
```
java -jar analyseJar.jar -c ./example/callstack/CVE-2016-9487@org.idpf:epubcheck:4.0.1 -j ./example/jar_to_analyse/epubcheck-4.0.1.jar -co

```
**<font color=red>output:</font>**
```
class soot.jimple.internal.JEqExpr;class soot.jimple.internal.JNeExpr;class soot.jimple.internal.JNeExpr;class soot.jimple.internal.JLeExpr;class soot.jimple.internal.JNeExpr;class soot.jimple.internal.JEqExpr;class soot.jimple.internal.JLeExpr;class soot.jimple.internal.JNeExpr;class soot.jimple.internal.JNeExpr;class soot.jimple.internal.JEqExpr;class soot.jimple.internal.JNeExpr;class soot.jimple.internal.JNeExpr;class soot.jimple.internal.JNeExpr;class soot.jimple.internal.JEqExpr;class soot.jimple.internal.JGtExpr;class soot.jimple.internal.JNeExpr;class soot.jimple.internal.JNeExpr
class soot.jimple.internal.JEqExpr;class soot.jimple.internal.JNeExpr;class soot.jimple.internal.JNeExpr;class soot.jimple.internal.JLeExpr;class soot.jimple.internal.JNeExpr;class soot.jimple.internal.JEqExpr;class soot.jimple.internal.JLeExpr;class soot.jimple.internal.JNeExpr;class soot.jimple.internal.JNeExpr;class soot.jimple.internal.JEqExpr;class soot.jimple.internal.JNeExpr;class soot.jimple.internal.JNeExpr;class soot.jimple.internal.JNeExpr;class soot.jimple.internal.JNeExpr;class soot.jimple.internal.JEqExpr;class soot.jimple.internal.JNeExpr
class soot.jimple.internal.JEqExpr;class soot.jimple.internal.JNeExpr;class soot.jimple.internal.JNeExpr;class soot.jimple.internal.JLeExpr;class soot.jimple.internal.JNeExpr;class soot.jimple.internal.JEqExpr;class soot.jimple.internal.JLeExpr;class soot.jimple.internal.JNeExpr;class soot.jimple.internal.JNeExpr;class soot.jimple.internal.JEqExpr;class soot.jimple.internal.JNeExpr;class soot.jimple.internal.JNeExpr;class soot.jimple.internal.JNeExpr;class soot.jimple.internal.JNeExpr;class soot.jimple.internal.JEqExpr;class soot.jimple.internal.JNeExpr;class soot.jimple.internal.JNeExpr;class soot.jimple.internal.JEqExpr
class soot.jimple.internal.JEqExpr;class soot.jimple.internal.JNeExpr;class soot.jimple.internal.JNeExpr;class soot.jimple.internal.JNeExpr;class soot.jimple.internal.JNeExpr
class soot.jimple.internal.JEqExpr;class soot.jimple.internal.JNeExpr;class soot.jimple.internal.JNeExpr;class soot.jimple.internal.JNeExpr
````````````

Example2:

This example takes para-core-1.17.1.jar as the input downstream jar, and CVE-2015-0886@org.mindrot:jbcrypt:0.3m@com.erudika:para-core:1.17 as the RI(Risky Invocation) file, with the cs analyse option, to get the call site information where RI happens. The output shows the information of all three RI specified at CVE-2015-0886@org.mindrot:jbcrypt:0.3m@com.erudika:para-core:1.17. The output pattern is guardt@return@parameter, which means whether the RI is guarded or not, whether the variable tainted by the RI is returned or not, whether the variable tainted by the RI is input to other functions as a parameter or not separately, and 1 refers to yes, 0 refers to no.

**<font color=green>command:</font>**
```
java -jar analyseJar.jar -c ./example/RI/CVE-2015-0886@org.mindrot:jbcrypt:0.3m@com.erudika:para-core:1.17 -j ./example/jar_to_analyse/para-core-1.17.1.jar -cs
```

**<font color=red>output:</font>**
```
1@1@0@
1@1@0@
1@0@1@
```