---
title: django ORM
tags: python django
comments: true
---

# 장고 ORM 정리

출처 : https://django-orm-cookbook-ko.readthedocs.io/en/latest/and_query.html

**실제 SQL질의문 확인하기**

    >>> queryset = Room.object.all()
    >>> str(queryset.query)
    SELECT "room_id", "room_name" FROM room

**or 연산 방법**

- queryset_1 | queryset_2
- filter(Q(cond_1) | Q(cond_2))

장고 orm코드

    queryset = Room.objects.filter(room_name__startswith='b') | Room.objects.filter(room_name__startswith='c')

Q 객체를 이용하는 방법

    from django.db.models import Q
    queryset = Room.objects.filter(Q(room_name__startswith='b')|Q(room_name__startswith='c'))


**And 연산방법**

    qs = Room.objects.filter(room_nm__startswith='a', room_nm__startswith='b')
    qs = Room.objects.filter(room_nm__startswith='a') & Room.objects.filter(room_nm__startswith='b')
    from django.db.models import Q
    qs = Room.objects.filter(Q(room_nm__startswith='a') & Q(room_nm__startswith='b'))


**부정연산**

    qs = Room.objects.exclude(id__lt=5)
    from django.db.models import Q
    qs = Room.objects.filter(Q(id__lt=5))


**union**

union 매서드는 합하려는 필드와 데이터 유형이 서로 일치할때만 실행이 가능하다.

    qs1 = Room.objects.filter(id__gte=8)
    qs2 = Room.objects.filter(id__lte=10)
    q1.union(q2)

    Cat.objects.all().values_list('name','breed')
    .union(Dog.objects.all().values_list('name','breed'))
