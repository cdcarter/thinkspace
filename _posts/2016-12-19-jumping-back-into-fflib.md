---
layout: post
title: "Jumping Back into fflib"
description: "enterprise patterns"
keywords: "salesforce"
---

```
public class BillingUOW extends fflib_SObjectUnitOfWork {
    public BillingUOW() {
        super(SERVICE_TYPES);
    }
    
    public void registerFieldUpdate(SObject record, Schema.sObjectField field, Object value){
        String sObjectType = record.getSObjectType().getDescribe().getName(); 
        if(!m_dirtyMapByType.containsKey(sObjectType))
            throw new UnitOfWorkException(String.format('SObject type {0} is not supported by this unit of work', new String[] { sObjectType }));
        if(!m_dirtyMapByType.get(sObjectType).containsKey(record.Id))
            registerDirty(record.getSObjectType().newSObject(record.Id));
        m_dirtyMapByType.get(sObjectType).get(record.Id).put(field, value);
    }
}
```

Ok...It'd a bit of a hack, but I extended the fflib UOW object a little bit. Instead of registering an object, we register a specific field update. The method takes a copy of the object with just nulls, and sets only the fields that we explicitly change.

This means throughout the domain class which is potentially calculating a lot of different fields but ideally only when a recalc is truly needed, we don't need to deal with making sure the *right* SObject only gets into the uow once. Like registerDirty() this has last-caller-wins semantics, but at the field level instead of the full object level.

This maybe isn't the neatest way of doing this. I may change this UOW to wrap the basic OUW instead of extend it.
