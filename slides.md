---
theme: seriph
info: |
  ## Make code readable again
title: Make code readable again!
layout: intro
---

# Make code readable again!

by Andrzej Wódkiewicz

Ramp Internal Tech Meet-up, 2022-05-11



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

---

# Good naming

Descriptive variables

```ts
const info = this.validate(data);
```

<v-click>

vs

</v-click>

<v-click>

```ts
const assetPriceInfo = this.validateResponse(assetPriceInfoResponse);
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

<v-click>

SRP

</v-click> 


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

```ts
private validateAddressAndAssignItToUser(dto: AddressDto) {
```

</v-click>

---

# Good naming

<v-click>

SRP (continued)

</v-click> 

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

- wrong conetxt
-->

---

# Behaviour goes first

<v-clicks>

- washes dishes -> dishwasher
- validates tickets -> ticket validator
- turns the build progress round and round for hours -> *Circle* CI 

</v-clicks>

---