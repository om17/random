org.mockito.exceptions.base.MockitoException: 

No tests found in UsageServiceImplTest
Is the method annotated with @Test?
Is the method public?


	at org.mockito.internal.runners.RunnerFactory.create(RunnerFactory.java:82)
	at org.mockito.internal.runners.RunnerFactory.createStrict(RunnerFactory.java:40)
	at org.mockito.junit.MockitoJUnitRunner.<init>(MockitoJUnitRunner.java:154)
	at java.base/java.lang.reflect.Constructor.newInstanceWithCaller(Constructor.java:502)
	at java.base/java.lang.reflect.Constructor.newInstance(Constructor.java:486)
	at java.base/java.util.stream.ReferencePipeline$3$1.accept(ReferencePipeline.java:197)
	at java.base/java.util.ArrayList$ArrayListSpliterator.tryAdvance(ArrayList.java:1685)
	at java.base/java.util.stream.ReferencePipeline.forEachWithCancel(ReferencePipeline.java:129)
	at java.base/java.util.stream.AbstractPipeline.copyIntoWithCancel(AbstractPipeline.java:527)
	at java.base/java.util.stream.AbstractPipeline.copyInto(AbstractPipeline.java:513)
	at java.base/java.util.stream.AbstractPipeline.wrapAndCopyInto(AbstractPipeline.java:499)
	at java.base/java.util.stream.FindOps$FindOp.evaluateSequential(FindOps.java:150)
	at java.base/java.util.stream.AbstractPipeline.evaluate(AbstractPipeline.java:234)
	at java.base/java.util.stream.ReferencePipeline.findFirst(ReferencePipeline.java:647)
Caused by: java.lang.reflect.InvocationTargetException
	at java.base/java.lang.reflect.Constructor.newInstanceWithCaller(Constructor.java:502)
	at java.base/java.lang.reflect.Constructor.newInstance(Constructor.java:486)
	at org.mockito.internal.runners.util.RunnerProvider.newInstance(RunnerProvider.java:29)
	at org.mockito.internal.runners.RunnerFactory.create(RunnerFactory.java:75)
	... 13 more
Caused by: org.junit.runners.model.InitializationError
	at org.mockito.internal.runners.DefaultInternalRunner$1.<init>(DefaultInternalRunner.java:31)
	at org.mockito.internal.runners.DefaultInternalRunner.<init>(DefaultInternalRunner.java:30)
	... 17 more
