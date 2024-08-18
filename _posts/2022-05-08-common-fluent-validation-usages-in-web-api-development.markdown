---
layout: post
title: "Common Fluent validation usages in web api development"
date: 2022-05-08 20:04:00 +0200
comments: true
published: true
categories: ["blog", "archives"]
tags: ["Fluent validation", "common fluentvalidation uses", "Validation for empty and null check for string field", "Validation for nullable type", "Validation based on When condition in a field", "Validation using custom rule and Validation context", "Validation for Email id using Regular Expression", "Validation using a static method call ", "Validation based on enum values", "Validation based on two fields in request model", "Validation based on external service method", "Validation for Child List items", "Validation with use of SetValidator", "Validation with conditional rule in a list of items", "FluentValidation in c#", "FluentValidation in asp.net", "FluentValidation in web api"]
permalink: /post/common-fluent-validation-usages-in-web-api-development
---
Validation is one of the essential parts of any api development, below will discuss some of the common usage scenarios of Fluentvalidation.

> _Note: The main idea of this post is to make you familiar with the different usage scenarios of the Fluentvalidation, so we will not discuss much on the dot net core project creation and solution structure. In this solution, we are using the Dependency injection concept, so you are assumed to be familiar with those concepts ahead._

For the demo purpose, I have created a simple web api project in dot net core. Below are the classes and objects used in the project.

*   Request Object **Customer**
*   One Service Method Class with Interface (ServiceMethod.cs)
*   One Validator Class (CustomerValidator.cs)
*   One Controller class (FluentValidationDemo.cs)
*   One Enum, Utility, and EnumExtension classes

### Prerequisite:

We have to add **FluentValidation.AspNetCore** package from Nuget.  
![](/assets/img/posts/2022/05/FluentValidationNuget.JPG)

Once the package is added we can see the reference below:  
![](/assets/img/posts/2022/05/FluentValidationNugetPackage.JPG)

We have created a dummy **customer** class object as below to demonstrate different scenarios.

```csharp
public class Customer
{
    public int Id { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string IdentificationNumber { get; set; }
    public string Email { get; set; }
    public string Gender { get; set; }
    public DateTime? DateOfBirth { get; set; }
    public string Mobile { get; set; }
    public string Username { get; set; }
    public List<Address> AddressList { get; set; }
    public List<Document> DocumentList { get; set; }
}

public class Address
{
    public string AddressLine1 { get; set; }
    public string AddressLine2 { get; set; }
    public string PostBox { get; set; }
}

public class Document
{
    public int Id { get; set; }
    public bool? IsActive { get; set; }
    public string DocumentName { get; set; }
    public DateTime ExpiryDate { get; set; }
    public DateTime CreatedDate { get; set; }
}
```

All validation functionality will be handled in the CustomerValidator class. From controller we will be calling the validator class as below.

`await new CustomerValidator(\_serviceMethods).ValidateAndThrowAsync(customer);`

Let us discuss different validation scenarios now:

1. ### Empty and Null check for a string field 
    Field: FirstName  
    Description: Checks the Firstname field for empty and null cases.  
    ValidationCode:  
    
    ```csharp
    RuleFor(x => x.FirstName)
            .NotEmpty()
            .WithMessage("First name required");
    ```
2. ### Empty check for nullable field  
    Field: DateOfBirth  
    Description: As you know dateofbirth is a nullable field, it checks DateOfBirth for value, please note that NotNull() check will not help in this case.  
    ValidationCode:  
    
    ```csharp
    RuleFor(x => x.DateOfBirth)
               .NotEmpty()
               .WithMessage("Date of birth required");
    ```

3. ### Validation based on Condition (using When)**  
    Field: DateOfBirth  
    Description: Check DateofBirth greater than today's date when it has value.  
    ValidationCode:  
    
    ```csharp
    When(e => e.DateOfBirth.HasValue, () =>
                {
                    RuleFor(model => model.DateOfBirth.Value.Date)
                        .LessThanOrEqualTo(x => System.DateTime.Today)
                        .WithMessage("Date of birth cannot be greater than todays date.");
                   
                });
    ```

4. ### Validation using custom rule and context**  
    Field: DateOfBirth  
    Description: Using DateofBirth we check the user is aged 18 years or above otherwise deny. The context object is used to pass the proper validation message  
    ValidationCode:  
    
    ```csharp
    RuleFor(model => model.DateOfBirth.Value.Date)
       .Custom((x, context) =>
           {
             if (x.AddYears(18) > DateTime.Now)
               {
                  context.AddFailure($"Users with age less than 18 years not allowed to register, please check DateofBirth {x:dd-MMM-yyyy}");
               }
           });
    ```

5. ### Validation using Regular Expression**  
    Field: Email  
    Description: The format of the email is checked using regular expressions.  
    ValidationCode:  
    
    ```csharp
    RuleFor(x => x.Email)
        .NotEmpty()
        .WithMessage("Email required")
        .Matches(@"^\[a-zA-Z0-9.!#$%&’\*+/=?^\_\`{|}~-\]+@\[a-zA-Z0-9-\]+(?:\\.\[a-zA-Z0-9-\]+)\*$")
        .WithMessage("Email format is wrong.");
    ```

6. ### Validation using Static method**  
    Field: Mobile  
    Description: The business logic is written in a static class, you can write your own logic in the method. In our example, we validate mobile number should start with 04 or 09 and has 10 digits.  
    ValidationCode:  
    
    ```csharp
    RuleFor(e => e.Mobile)
        .NotEmpty()
        .WithMessage("Mobile number required.")
        .Must(x => Utility.ValidateMobileNumber(x))
        .WithMessage("Mobile number Not Valid");
    
    
    public static bool ValidateMobileNumber(string userMobile)
      {
        bool isValidMobileNumber = false;
         if (!string.IsNullOrEmpty(userMobile))
             isValidMobileNumber = new Regex(@"^0(9|4)\\d{8}$").IsMatch(userMobile);
         return isValidMobileNumber;
      }
    ```

7. ### Validation based on items in enum List**  
    Field: Gender  
    Description: Here we use an enum extension function to get the list of enum descriptions and validate whether the gender field value is present in that list when the gender field is not empty.  
    ValidationCode:  
    
    ```csharp
    //get list of gender types
    readonly List<string> genderTypes = EnumExtension.GetEnumValuesAndDescriptions<GenderTypes>().Select(c => c.Value).ToList();
    
    //validation
    When(e => !string.IsNullOrEmpty(e.Gender), () =>
        {
           RuleFor(e => e.Gender)
               .Must(x => genderTypes.Contains(x.ToUpper()))
               .WithMessage($"Gender is invalid, allowed values are {String.Join(",", genderTypes)}");
       });
    
    //enum 
    public enum GenderTypes
            {
                \[Description("MALE")\]
                MALE,
                \[Description("FEMALE")\]
                FEMALE,
                \[Description("OTHER")\]
                OTHER
            }
    
    //enum extesion method
    public static List<KeyValuePair<string, string>> GetEnumValuesAndDescriptions<T>()
            {
                Type enumType = typeof(T);
                if (enumType.BaseType != typeof(Enum))
                    throw new ArgumentException("T is not System.Enum");
                List<KeyValuePair<string, string>> enumValList = new List<KeyValuePair<string, string>>();
                foreach (var e in Enum.GetValues(typeof(T)))
                {
                    var fi = e.GetType().GetField(e.ToString());
                    var attributes = (DescriptionAttribute\[\])fi.GetCustomAttributes(typeof(DescriptionAttribute), false);
                    enumValList.Add(new KeyValuePair<string, string>(e.ToString(), (attributes.Length > 0) ? attributes\[0\].Description : e.ToString()));
                }
                return enumValList;
            }
    ```

8. ### Validation based on two fields in the request model**  
    Field: Username, Email  
    Description: Here we check if the email field contains username text when both fields are not empty.  
    ValidationCode:  
    
    ```csharp
    When(e => (!string.IsNullOrEmpty(e.Username) && !string.IsNullOrEmpty(e.Email)), () =>
                {
                    //Validation based on two fields in request model
                    RuleFor(e => new {e.Username,e.Email })
                            .Must(x => !x.Email.Contains(x.Username))
                            .WithMessage(x=>$"Email should not contain the username: {x.Username}");
                });
    ```

9. ### Validation based on service method using Dependency Injection**  
    Field: IdentificationNumber  
    Description: Here we validate IdentificationNumber matching certain criteria, we can write our own business logic in that method, and we can also using this method for any database related checks. We inject the service interface into the constructor of the validation class.  
    ValidationCode:  
    
    ```csharp
     RuleFor(e => e.IdentificationNumber)
         .NotEmpty()
         .WithMessage("Identification number required.")
         .MustAsync(async (f, \_) => await \_serviceMethods.ValidateIdentificationNumber(f))
         .WithMessage("Identification number not valid");
    ```

10. ### Validation for each child List items**  
    Field: AddressList.PostBox  
    Description: Here we validate postbox field inside the list object AddressList. Each address object should have mandatory postbox text.  
    ValidationCode:  
    
    ```csharp
    RuleForEach(x => x.AddressList).ChildRules(items =>
        {
           items.RuleFor(e => e.PostBox)
             .NotEmpty()
             .WithMessage("Postbox required.");
        });
    ```

11. ### Validation for List of Items using SetValidator**  
    Field: DocumentList  
    Description: Here we validate documentList using a separate validator method. We use this approach when documentList object is used in multiple places in the application, so all validation for this object remains in one place.  
    ValidationCode:  
    
    ```csharp
    When(e => (e.DocumentList != null && e.DocumentList.Count > 0), () =>
            {
                RuleForEach(x => x.DocumentList)
                    .SetValidator(model => new DocumentValidator());
            });
    
    //DocumentValidator Class
    public class DocumentValidator : AbstractValidator<Document>
        {
            public DocumentValidator()
            {
    
                RuleFor(x => x.DocumentName)
                   .NotEmpty()
                   .WithMessage("Document name required");
    
                RuleFor(x => x.CreatedDate)
                        .LessThan(x => System.DateTime.Today)
                        .WithMessage("Document created date cannot be greater than todays date.");
    
                RuleFor(x => x.ExpiryDate)
                      .GreaterThan(x => System.DateTime.Today)
                      .WithMessage("Document expiry date should be greater than todays date.");
    
                RuleFor(x => x.ExpiryDate)
                     .GreaterThan(x => x.CreatedDate)
                     .WithMessage("Document expiry date should be greater than created date.");
            }
        }
    ```

12. ### Validation based on the conditional rule in List of Items**  
    Field: DocumentList  
    Description: Here we validate items in documentList which are having IsActive value as true. This validation is closely similar to method 10 mentioned above, the only difference is that we need to add OverridePropertyName since there is a condition in the list of items.  
    ValidationCode:  
    
    ```csharp
    RuleForEach(model => model.DocumentList.Where(g => g.IsActive.HasValue  && g.IsActive.Value)).ChildRules(items =>
        {
           items.RuleFor(x => x.Id)
                 .NotEmpty()
                 .WithMessage("DocumentList Id required when document active.");
        }).OverridePropertyName("DocumentListValidatorWhenActive");
    ```              
The complete source code of CustomerValidator and Document Validator is as below.

```csharp
public class CustomerValidator : AbstractValidator<Customer>
    {
        readonly List<string> genderTypes = EnumExtension.GetEnumValuesAndDescriptions<GenderTypes>().Select(c => c.Value).ToList();
        public CustomerValidator(IServiceMethod \_serviceMethods)
        {
            //Validation for empty and null check for string field
            RuleFor(x => x.FirstName)
                .NotEmpty()
                .WithMessage("First name required");

            // Validation for nullable type
            RuleFor(x => x.DateOfBirth)
                .NotEmpty()
                .WithMessage("Date of birth required");

            //Validation based on When condition in a field
            When(e => e.DateOfBirth.HasValue, () =>
            {
                RuleFor(model => model.DateOfBirth.Value.Date)
                    .LessThanOrEqualTo(x => System.DateTime.Today)
                    .WithMessage("Date of birth cannot be greater than todays date.");

                //Validation using custom rule and Validation context
                RuleFor(model => model.DateOfBirth.Value.Date)
                 .Custom((x, context) =>
                 {
                     if (x.AddYears(18) > DateTime.Now)
                     {
                         context.AddFailure($"Users with age less than 18 years not allowed to register, please check DateofBirth {x:dd-MMM-yyyy}");
                     }
                 });

            });

            // Validation for Email id using Regular Expression
            RuleFor(x => x.Email)
                .NotEmpty()
                .WithMessage("Email required")
                .Matches(@"^\[a-zA-Z0-9.!#$%&’\*+/=?^\_\`{|}~-\]+@\[a-zA-Z0-9-\]+(?:\\.\[a-zA-Z0-9-\]+)\*$")
                .WithMessage("Email format is wrong.");

            //Validation using a static method call            
            RuleFor(e => e.Mobile)
                .NotEmpty()
                .WithMessage("Mobile number required.")
                .Must(x => Utility.ValidateMobileNumber(x))
                .WithMessage("Mobile number Not Valid");

            //Validation based on enum values with description, gender is optional value
            When(e => !string.IsNullOrEmpty(e.Gender), () =>
            {
                RuleFor(e => e.Gender)
                        .Must(x => genderTypes.Contains(x.ToUpper()))
                        .WithMessage($"Gender is invalid, allowed values are {String.Join(",", genderTypes)}");
            });

            
            When(e => (!string.IsNullOrEmpty(e.Username) && !string.IsNullOrEmpty(e.Email)), () =>
            {
                //Validation based on two fields in request model
                RuleFor(e => new {e.Username,e.Email })
                        .Must(x => !x.Email.Contains(x.Username))
                        .WithMessage(x=>$"Email should not contain the username: {x.Username}");
            });

            //Validation based on external service method
            RuleFor(e => e.IdentificationNumber)
                .NotEmpty()
                .WithMessage("Identification number required.")
                .MustAsync(async (f, \_) => await \_serviceMethods.ValidateIdentificationNumber(f))
                .WithMessage("Identification number not valid");

            //Validation for Child List items
            RuleForEach(x => x.AddressList).ChildRules(items =>
            {
                items.RuleFor(e => e.PostBox)
                 .NotEmpty()
                 .WithMessage("Postbox required.");
            });

            //Validation with use of SetValidator seperate class
            When(e => (e.DocumentList != null && e.DocumentList.Count > 0), () =>
            {
                RuleForEach(x => x.DocumentList)
                    .SetValidator(model => new DocumentValidator());
            });

            //Validation with conditional rule in a list of items
            RuleForEach(model => model.DocumentList.Where(g => g.IsActive.HasValue
            && g.IsActive.Value)).ChildRules(items =>
            {
                items.RuleFor(x => x.Id)
                    .NotEmpty()
                    .WithMessage("DocumentList Id required when document active.");
            }).OverridePropertyName("DocumentListValidatorWhenActive");
        }
    }


    public class DocumentValidator : AbstractValidator<Document>
    {
        public DocumentValidator()
        {

            RuleFor(x => x.DocumentName)
               .NotEmpty()
               .WithMessage("Document name required");

            RuleFor(x => x.CreatedDate)
                    .LessThan(x => System.DateTime.Today)
                    .WithMessage("Document created date cannot be greater than todays date.");

            RuleFor(x => x.ExpiryDate)
                  .GreaterThan(x => System.DateTime.Today)
                  .WithMessage("Document expiry date should be greater than todays date.");

            RuleFor(x => x.ExpiryDate)
                 .GreaterThan(x => x.CreatedDate)
                 .WithMessage("Document expiry date should be greater than created date.");
        }
    }

  ```
The full source code is in git [FluentValidationDemo](https://github.com/lijotech/FluentValidationDemo/tree/master "FluentValidationDemo")

### Summary

We have seen twelve commonly needed use cases of Fluent validation in asp.net core. There are more use cases and scenarios for using this powerful library, you can visit the official documentation of Fluentvalidation for that [docs](https://docs.fluentvalidation.net/en/latest/aspnet.html "fluentvalidation documentation").