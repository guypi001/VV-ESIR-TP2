# No getter!

With the help of JavaParser implement a program that obtains the private fields of public classes that have no public getter in a Java project. 

A field has a public getter if, in the same class, there is a public method that simply returns the value of the field and whose name is `get<name-of-the-field>`.

For example, in the following class:

```Java

class Person {
    private int age;
    private String name;
    
    public String getName() { return name; }

    public boolean isAdult() {
        return age > 17;
    }
}
```

`name` has a public getter, while `age` doesn't.

The program should take as input the path to the source code of the project. It should produce a report in the format of your choice (TXT, CSV, Markdown, HTML, etc.) that lists for each detected field: its name, the name of the declaring class and the package of the declaring class.

Include in this repository the code of your application. Remove all unnecessary files like compiled binaries. See the [instructions](../sujet.md) for suggestions on the projects to use.

*Disclaimer* In a real project not all fields need to be accessed with a public getter.
#Answer
__________________________________________________________________________________________________________________________________________________

```Java
	package parserprog;

	import com.github.javaparser.StaticJavaParser;
	import com.github.javaparser.ast.CompilationUnit;
	import com.github.javaparser.ast.body.BodyDeclaration;
	import com.github.javaparser.ast.body.ClassOrInterfaceDeclaration;
	import com.github.javaparser.ast.body.MethodDeclaration;
	import com.github.javaparser.ast.expr.BinaryExpr;
	import com.github.javaparser.ast.expr.Expression;
	import com.github.javaparser.ast.visitor.VoidVisitorAdapter;
	import com.google.common.base.Strings;

	import java.io.File;
	import java.io.IOException;
	import java.util.stream.Collectors;

	public class NoGetter {


		public static void listClasses(File projectDir) {
		new DirExplorer((level, path, file) -> path.endsWith(".java"), (level, path, file) -> {
		    System.out.println(path);
		    System.out.println(Strings.repeat("=", path.length()));
		    try {
			new VoidVisitorAdapter<Object>() {
			    @Override
			    public void visit(ClassOrInterfaceDeclaration n, Object arg) {
				super.visit(n, arg);
				System.out.println(" * " + n.getName());

					int nbr_champs = n.getFields().size();
					int nbr_methods = n.getMethods().size();
					boolean bb = false;
					for(int i = 0; i< nbr_champs; i++) {
						if( !n.getFields().get(i).isPrivate() ) 
							continue;
						String name = n.getFields().get(i).getVariable(0).toString();
						String type = n.getFields().get(i).getElementType().toString();
						for(int j = 0; j< nbr_methods; j++) {
							String method = n.getMethods().get(j).getName().toString();
							if(method.substring(3).toLowerCase().equals(name) && n.getMethods().get(j).isPublic() && n.getMethods().get(j).getType().toString().equals(type) && n.getMethods().get(0).getBody().get().getStatements().size() == 1) 
								continue;
							bb = true;
						}
					}
					if(bb)
						System.out.println("No getter");
			    }
			}.visit(StaticJavaParser.parse(file), null);
			System.out.println(); // empty line
		    } catch (IOException e) {
			throw new RuntimeException(e);
		    }
		}).explore(projectDir);
	    }
	    
```
