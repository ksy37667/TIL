# 🔍트랜잭션(Transaction) 이란?
* 데이터베이스의 상태를 변화시키기 위한 작업의 단위를 의미한다. 트랜잭션은 간단히 말해서 작업의 완전성을 보장해주는 것이다. 트랜잭션의 작업 셋을 완벽하게 처리하지 못할 경우 다시 원상태로 복구해서 작업 셋의 일부만 적용되는 것을 막기 위한 기능이다.

# 🔍Django에서 간단한 트랜잭션을 사용해보자.

* 상품주문을 등록하는 RegisterForm 클래스이다.

```python
#........................ 

class RegisterForm(forms.Form):
    #............................
    def clean(self):

        # .......
        # .......

        if quantity and product and shopuser:
            prod = Product.objects.get(pk=product)
            order = Order(
                quantity = quantity,
                product=Product.objects.get(pk=product),
                shopuser=Shopuser.objects.get(email=shopuser)
            )
            order.save()
            prod.stock -= quantity
            prod.save()
        else:
            #.................
            #.................
```
* 상품주문을 등록할 때 `quantity` 의 값이 주문정보 테이블에 저장이 됐다면 당연히 재고의 양을 저장한 데이터의 갱신도 이루어져야 하기 때문에 transaction 을 사용하여 두개의 작업을 하나의 단위로 묶어야한다.

* 먼저 아래처럼 transaction을 import 해준다.
```python
from django.db import transaction
```

* `with transaction.atomic()` 으로 하나의 트랜잭션으로 동작할 수 있게 감싼다.
```python

```python
#........................ 
from django.db import transaction

class RegisterForm(forms.Form):
    #............................
    def clean(self):

        # .......
        # .......

        if quantity and product and shopuser:
            with transaction.atomic():
                prod = Product.objects.get(pk=product)
                order = Order(
                    quantity = quantity,
                    product=Product.objects.get(pk=product),
                    shopuser=Shopuser.objects.get(email=shopuser)
                )
                order.save()
                prod.stock -= quantity
                prod.save()
        else:
            #.................
            #.................
```
* 이러한 방식으로 데이터베이스의 관련된 동작들을 하나의 트랜잭션으로 처리 할 수 있다.

