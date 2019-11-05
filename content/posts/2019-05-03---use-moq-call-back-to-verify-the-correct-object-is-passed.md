---
title: Use Moq CallBack to Verify Object Parameter
date: "2019-05-03T00:00:00.000Z"
template: "post"
category: "Programming"
tags: 
  - "Unit test"
draft: false
slug: "/posts/use-moq-call-back-to-verify-the-correct-object-is-passed/"
description: "Our repositories accept domain models as parameter to save them to the database. Moq's verify works greatly if the method has primitive types like string, int, and bool, not so good at showing error message if the paramter is an object."
socialImage: "/media/42-line-bible.jpg"

---

Our repositories accept domain models as parameter to save them to the database. Moq's verify works greatly if the method has primitive types like string, int, and bool, not so good at showing error message if the paramter is an object. 

```csharp
_api.CustomerRepositoryMock.Verify(r => r.UpdateCustomer(It.Is<Customer>(c => 
  c.CustomerId == customerId &&
  c.CompanyName == companyName &&
  ...
)));
```

if any one of the properties fail to match the expected value, like FirstName doesn't match fistName, Moq's verify doesn't tell you that's the error. It just simpley says the expected method with the given paramters are not invoked. To track down the failing match, you have to guess or to debug, which is annying. 

CallBack is very handy in this case. By setting up CallBack to your repoisitory, you can get the passed parameter object back.

```csharp
_api.CustomerRepositoryMock.Setup(r => r.UpdateCustomer(It.IsAny<Customer>()))
.Callback<BusinessCustomer>(c => customerToUpdate = c);

```

Once you have the object, you can write multiple assert statements and any failing match will tell you what fails and what's the actual value.

```csharp
// Assert
Assert.Equal(customerId, customerToUpdate.CustomerId);
Assert.Equal(expectedCompanyName, customerToUpdate.CompanyName);

```