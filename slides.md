---
theme: seriph
info: |
  ## Make code readable again
title: Make code readable again!
layout: intro
---

# Make code readable again!

by Andrzej Wódkiewicz

<v-clicks>

Subtitle:

Opinionated guide on how to write code that is most likely bad advice anyway

but at least you can argue it's correct cause it was presented on a meetup

Alternative subtitle:

I'm sick of reading your code, please do it my way


</v-clicks>

> Ramp Internal Tech Meet-up, 2022-05-11


---
layout: center
---

# Why?

---
layout: statement
---


We *read* code more than we *write* it

<!--
Optimize the code for reading, 
it's harder to read code than to write it
-->

---
layout: statement
---

It's easier to work independently on readable code

<!--
As an author make sure that nobody will ever try to contact you about the things you wrote.
Ever. 
-->

---
layout: statement
---

It's easier to review readable code

<!--
Make a brand out of your work.
Do such a good job that your colleagues will not want to review anybody else's code ever again. 
Other people will review your work faster... *click*
-->


---
layout: statement
---

It's easier to spot bugs

<!--
...and find potential bugs quicker. Maybe you'll even find them yourself before you create a PR.
-->


---
layout: center
---

# How?

---
layout: section
---

# Good naming

<!--
Let's talk about javascript
-->

---

# Good naming

Descriptive variables

```ts
const info = this.handle(data);
```

<v-click>

vs

</v-click>

<v-click>

```ts
const assetPriceInfo = this.validateApiResponse(rawApiResponse);
```

</v-click>




<v-click>

<br>
<hr>

don't you ever dare:

```ts

const isTrue = (x && y) ? obj.ok : false;

```

</v-click>

<v-click>

🤡👏🤡👏🤡👏

</v-click>

<!--
Maybe if you look at the whole class context it makes sense.

But why making this assumption -- wouldn't it be easier if we just wrote what we actually mean?
-->

---

# Good naming

Add context

<v-click>

```ts
function applyFee(amount: number) {}
```

</v-click>

<v-click>

```ts
const amount = this.calculateAmount();
applyFee(amount);
```

</v-click>

<v-click>

```ts
private calculateAmount() {
  // FIXME This is only temporary, calculate actual fee at some point
  return this.dynamicConfig.getDouble('GLOBAL_MIN_FEE') ?? 0.1;
}
```

</v-click>


<v-click>

😡😡😡😡😡

</v-click>

<!--
we apply some fee
so far so good

what is the amount? let's find out the usages
-->

---

# Good naming

Add context 

<v-click>

```ts
function applyPaymentMethodFee(amountInEur: number) {
```

</v-click>

<v-click>

```ts
const amountInEur = this.calculateAmountInEur();
applyPaymentMethodFee(amountInEur);
```

</v-click>

<v-click>

```ts
const timeout = 1000;
const timeoutInMs = 1000;
```

</v-click>

---
# Good naming

Named predicates

```ts
if (Boolean(!rolePerKey[lookForKey]?.includes(safeUserRole))) {
   throw new HttpException('Forbidden', HttpStatus.FORBIDDEN);
}
```

<v-click>

vs

</v-click>

<v-click>

```ts
const isUserRoleNotSufficient = Boolean(!rolePerKey[lookForKey]?.includes(safeUserRole));
if (isUserRoleNotSufficient) {
   throw new HttpException('Forbidden', HttpStatus.FORBIDDEN);
}
```

</v-click>

<v-click>

vs

</v-click>

<v-click>

```ts
const isUserAuthorized = Boolean(authorizedLevels[featureName]?.includes(userLevel));
if (!isUserAuthorized) {
  throw new HttpException('Forbidden', HttpStatus.FORBIDDEN);
}
```

</v-click>

---

# Good naming


Obeying SRP


<v-click>

```ts
{
  this.prepareAddress(addressDto);
  this.complianceService.userDataChanged(request.userId, { type: "NEW_ADDRESS", address });
}
```

</v-click>

<v-click>

```ts {all|6-7|all}
private prepareAddress(dto: AddressDto) {
  const address: Address | undefined = this.cityMap.getAddress(dto);
  if (!address) {
    return;
  }
  const user = this.userRepo.find(dto.userId);
  user.assignAddress(address);
}
```

</v-click>


<v-click>

actually 


```ts
private validateAddressAndAssignItToUser(dto: AddressDto) {
```

</v-click>

---

# Good naming


Obeying SRP


<v-click>

```ts
{
  this.prepareAddress(addressDto);
  this.complianceService.userDataChanged(request.userId, { type: "NEW_ADDRESS", address });
}
```

vs

```ts
{
  const address = this.validateAddress(dto);
  if (!address) {
    return;
  }
  this.assignAddressToUser(address, dto.userId);
  this.complianceService.userDataChanged(dto.userId, { type: "NEW_ADDRESS", address });
}
```

</v-click>

---

# Behaviour goes first

<v-click>

- How does **it** behave, what does **it** do?
- What are the rules of the behaviour, how does **it** do those things?
- What is **it**?

</v-click>

<!-- 
Names convey some meaning.

- unknown responsiblity

- wrong context

Before you put some tag on a particular class, try to find out what it does, how it does and only then try to apply a name.
-->

---

# Behaviour goes first

<v-clicks>

- washes dishes -> dishwasher
- validates tickets -> ticket validator
- does an API call that fetches JSON data -> XMLHttpRequest
- turns the build progress round and round for hours -> *Circle* CI 

</v-clicks>

<v-clicks>

- Helper
- Manager
- Service

</v-clicks>

---

# Polish your English

<div v-click-bhide>

```ts
  // TODO: add some ridiculous examples before the presentation
```

</div>

<v-clicks>

givenRequestMakeOk 

"making requests is ok"?

"request was made and it's ok"?

</v-clicks>

---

# Convention

Don't use concepts with existing meaning in a different way

---
layout: statement
---

Be the best version of yourself

---
layout: statement
---

Write as if you needed to come back to the code and refactor it a year later.


<v-clicks>

It's going to be shit anyway.

</v-clicks>