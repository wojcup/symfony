# Form Options Variables Finishview

We know how powerful these form variables can be and we know that we can pass variables in one [inaudible 00:00:08] field. Here's my question: Can we control these variables from somewhere inside of our form class? Now remember, I mentioned earlier that the options for a field are different than the variables for a field. In a few cases they're the same like there's a place-holder option, and there's actually a place-holder variable, but for the most part they're different. I'm going to show you how that works behind the scenes and then we're going to build in and make our help feature more powerful.

Behind every single field type is a class. Obviously for the sub-family it's this entity type class, but for the name type this is actually defaulting to the text type class. I'm actually going to use shift shift inside of my editor and I'm going to open up a class called text type from the symfony form component.

Now, unlike variables, when you pass options into a class there's only a finite number of valid options. If you pass an option that doesn't exist you're going to get a big error. The way these options are determined are by the configure options method on your fields class. Obviously in the text type there is a component option that's allowed.

Inside the form component there's actually a little of inheritance going on. It's not real inheritance as you see text type extends something called abstract type, but behind the scenes text all fields ultimately extend a class called form type. I want you to open up that class as well. What's cool about this is it also has a configure option method and it adds a whole bunch of other options. If you're wondering where all the options come from, why do we have all these options and what do they do, you can actually look inside of your fields type like text type and also its parent type and sometimes you can figure out what these options are doing. Actually this class extends another class called base type and it has even more fields down here.

Now, we know that there is both a label option, we're not using it here but we could pass label and as an option and a label variable inside of our trig template. If I set a label option on sub-family that will become the label variable inside our trig template. The way that happens is really, really simple. In configure options inside of the base type, label=>no allows us to pass on the label option.

Then these classes also have another function called build view. What happens is, inside of your controller when you call create view, that turns your form object into a form view object. In fact, your form is kind of a big tree. You have a form on top and you have sub-fields that are also form objects. When you call create view, that top level form object is turned into a form view and all these sub-fields are also turned into form view objects.

When that happens the build view method is called an each individual field. One of the arguments that build view is actually the options that you pass to that field. For example, for the sub-family it would pass on these 3 options. If you also pass on the label option then that would be passed here. If you follow the logic down a little bit, eventually this function sets our VARS and in a lot of cases it does it based off the options.

I know it's a little bit confusing, but in essence every field has a number of options and sometimes those options are used to set the VARS on there. That's just for convenience so that you can set the label on the form class or you can set the label inside of your twig template.

Close up those classes and I'm going to ask a very simple question. I know I set a help variable inside of my twig template, but could I set that help variable inside of my form class? The answer is yes. The way to do it is by overriding another method inside this class called finished view. Either go to code it generates or Command N, it's like override methods and select finish view. There's actually two methods that are called when your form has turned into a form view object. They're subtly different so don't worry about it, but in this case you want finish view. You don't need to call the parent function.

Now check this out. We can say view [ and let's set the funfact field. Fun fact>bars>help= then we'll type a message. For example, "leatherback sea turtles can travel more than 10,000 miles every year." Let's break this down. When this form class has turned into a form view, Symfony calls this finished view function at the end. Remember, our form is a big tree. Our form view on top has sub-form views for each field. The way you can access that is by saying view[funfact.

In fact, inside of your form template when you say genusform.funfact, genus form is a form view object, it may seem when you say .funfact that actually reads the funfact array key off of that object. In other words, those two syntaxes are exactly the same. Then we say >vars which is a public array property and we add a help variable to it. All in all when we're done this sets the view variable.

Now it's up to one level difficulty bigger. I'm going to copy my stream but then comment this function out because here's what I want to be able to do. I want to be able to go up to my funfact field past no as the second argument so it keeps guessing my form type and then pass it in a help option. I want this to ultimately set a help variable. If I did this now, huge error because as I mentioned earlier, there is no option called help. It's invalid to pass that. I want to make this work for every field in my system. Let's do it.