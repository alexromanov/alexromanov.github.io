---
layout: post
title:  "The one with custom transformer for Cucumber in Scala"
date:   2016-07-28 10:09:45 +0300
author: "Alexander Romanov"
description: "In the following post I will show a quick tip on now you can write a custom transformer with Scala and Cucumber tool"

---

_In the following post I will show a quick tip on now you can write a custom transformer in order to parse decimal value from your step_  

Intellij IDEA 2016.2 CE is used as IDE

**Target audience:** QA Automation engineers / developers

DISCLAIMER: It is assumed that reader has some experience with Behavior Driven Development approach and
a Cucumber test tool. For full information please refer to official [Cucumber website][cucumber-site].

### A little bit of theory

By default Cucumber recognizes only two possible parameters which come from your steps: strings (single and double-quoted) and integers. 

If Cucumber parser sees strings:

{% highlight gherkin %}
  Scenario: Should transform strings by default
    Then Cucumber should parse "double quoted" as string parameter
{% endhighlight %}

Then it will propose to the user the generated code with sample parameters of type String:

{% highlight scala %}
  Then("""^Cucumber should parse "([^"]*)" as string parameter$"""){ (arg0:String) =>
    //// Write code here that turns the phrase above into concrete actions  

    throw new PendingException()
  }
{% endhighlight% %}

The same is for integer values in scenario step:

{% highlight gherkin %}
  Scenario: Should transform integers by default
    Then Cucumber should parse 1 and 123123 as integer parameters
{% endhighlight %}

And as a result:

{% highlight scala %}
  Then("""^Cucumber should parse (\d+) and (\d+) as integer parameters$"""){ (arg0:Int, arg1:Int) =>  
    //// Write code here that turns the phrase above into concrete actions  

    throw new PendingException()
  }
{% endhighlight %}

**NOTE:** Pay attention on how Cucumber generates regex for strings and integers by default.  

### What if you want something more complex?
There are a lot of situations when complex data should be used as a parameter in scenario steps.
It can be for example decimal value or your custom data type.   
In [Cucumber for Java book][cucumber-for-java] can be found a well described example of getting Money object as a step parameter. Specific example is written in Java and can be found [here][java-cucumber-transformer-usage].  
The main point is to write your custom class which extends Cucumber's [Transformer][cucumber-transformer] and then use it step implementations as annotation for specific parameter. 

{% highlight java %}
import cucumber.api.Transformer;
import nicebank.Money;
public class MoneyConverter extends Transformer<Money> {
    public Money transform(String amount) {
        String[] numbers = amount.split("\\.");
        int dollars = Integer.parseInt(numbers[0]);
        int cents = Integer.parseInt(numbers[1]);
        return new Money(dollars, cents);
    }
}
{% endhighlight %}

### Okay, but can we write it on Scala? 
In Scala language we can write our custom transformer as well. It is a little bit trickier, but pretty easy.  

For example we need to parse decimal value and use it our step

{% highlight gherkin %}
  Scenario: Should transform decimal with custom transformer
    Then Cucumber should parse 1.2 with custom transformer
{% endhighlight %}

When you run scenario, logs will show that the value 1.2 is transformed by Cucumber as two integer parameters with a point delimeter.

{% highlight scala %}
  Then("""^Cucumber should parse (\d+)\.(\d+) with custom trnaformer$"""){ (arg0:Int, arg1:Int) =>
    //// Write code here that turns the phrase above into concrete actions  

    throw new PendingException()
  }
{% endhighlight %}

Here are the steps for implementing your custom transformer for BigDecimal value

### 1. Create your custom trnasformer class for BigDecimal values  

{% highlight scala %}

import cucumber.api.{Transformer}
import cucumber.deps.com.thoughtworks.xstream.annotations.XStreamConverter

class MyBigDecimalTransformer extends Transformer[MyBigDecimal] {
  override def transform(value: String): MyBigDecimal = {
    MyBigDecimal(BigDecimal(value))
  }
}

@XStreamConverter(classOf[MyBigDecimalTransformer])
case class MyBigDecimal(value: BigDecimal) {
}

{% endhighlight %}

**NOTE:** Cucumber for Scala does not recognize native scala.math.BigDecimal values (and in fact throws an exception that custom transformer class should be implemented) - as a result we should implement a basic wrapper for desired type (or your own custom type) with XStreamConverter annotation.  

### 2. Use custom transformer in Step Definitions

{% highlight scala %}
  Then("""^Cucumber should parse ([\d\.]*) with custom transformer$"""){ (decimalParameter: MyBigDecimal  
  @Transform(classOf[MyBigDecimalTransformer])) =>
    assert(decimalParameter.value == 1.2, "Unexpected result for decimal parameter")
  }
{% endhighlight %}  

**NOTE:** Pay attention on regular expression for single decimal value "([\d\.]*)" and also for usage your custom MyBigDecimal wrapper with Transform annotation.

For now you can write transformer for many custom data types in your Cucumber steps. 

[cucumber-site]: https://cucumber.io/
[cucumber-for-java]: https://amzn.com/1941222293
[java-cucumber-transformer-usage]: https://media.pragprog.com/titles/srjcuc/sketching.pdf
[cucumber-transformer]: https://cucumber.github.io/api/cucumber/jvm/javadoc/cucumber/api/Transformer.html
