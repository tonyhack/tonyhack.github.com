---
layout: post
category : thrift
title: Thrift中的enum类型
description: Thrift中的enum类型不能嵌套使用
tags : [thrift, enum]
---

Thrift是facebook的开源RPC库，对于网络方面，我已有自己还算成熟的网络库，所以我把Thrift当作一个序列化库使用
（为什么不使用google-protobuffer？那是因为Thrift的官方多语言支持）。

在使用Thrift的过程中，遇到了一些问题，就例如enum类型不能嵌套使用。其实enum不能嵌套使用是挺不方便的，
前几天在做游戏服务器端时就遇到这个问题。

因为游戏中有一些实体（如玩家、npc、宠物等），这些实体有很多属性（血量、法力、速度等），而这些属性需要被各种
子模块（如技能、任务、AI等）存取，最重要的一点是要被脚本进行存取，所以我把这些实体定义为以下形式。

{% highlight c++ %}

struct FieldsType {
  enum type {
    HP = 0,
    MAXHP,
    POWER,
    MAXPOWER,
    SPEED,

    MAX,
  };
};

class Role {
 public:
  Role();
  ~Role();

  // Get and set.
  core::uint32 GetAttribute(core::uint16 type);
  void SetAttribute(core::uint16 type, core::uint32 value);

 private:
  core::uint32 fields_[FieldsType::MAX];
};

{% endhighlight %}

而对于这种方式的属性，子模块进行访问就很方便了。

{% highlight c++ %}

Skill::Attack(core::uint64 src_id, core::uint64 dest_id,
      core::uint32 skill_id) {
  Role *src_role = RoleManager::GetInstance().GetRole(src_id);
  Role *dest_role = RoleManager::GetInstance().GetRole(src_id);
  if(src_role && dest_role) {
    core::uint32 hurt_value = this->GetBasicHurt(skill_id);
    dest_role->SetAttribute(FieldsType::HP,
        dest_role->GetAttribute(FieldsType::HP) - hurt_value);
  }
}

{% endhighlight %}

但是对于脚本（通常指Lua或Python）来说，对实体的属性进行存取就存在一个问题，那就是FieldsType要在脚本里重新定义一次，
在研发中可能需要经常去改变这个定义，所以使用thrift来定义，然后生成相应语言的代码即可。

而我们的实体是在场里活动的，所以实体有一些可见属性和非可见属性，所以我们之前的定义就需要改变，使用thrift来定义。

{% highlight c++ %}

namespace cpp game.server.entity

enum FieldsType {
  // 可见属性
  PUBLIC_BEGIN = 0,

  HP = AIO_BEGIN,
  MAXHP,
  POWER,
  MAXPOWER,

  PUBLIC_END,


  // 不可见属性
  PRIVATE_BEGIN = PUBLIC_END,

  SPEED = PRIVATE_BEGIN,

  PRIVATE_END,
}

{% endhighlight %}

定义以上这种类型，可以满足我们的要求，但是在thrift生成时，确出错了，原因是enum中出现了嵌套定义，我们只能改变现有做法。

{% highlight c++ %}

namespace cpp game.server.entity

enum RolePublicFields {
  PUBLIC_BEGIN = 0,

  HP = 0,
  MAXHP,
  POWER,
  MAXPOWER,

  PUBLIC_END,
}

enum RolePrivateFields {
  PRIVATE_BEGIN = 0,

  SPEED = 0,

  PRIVATE_END,
}

{% endhighlight %}

改变我们的Role定义。

{% highlight c++ %}

class Role {
 public:
  Role();
  ~Role();

  // Get and set.
  // 这里把public与private属性的存取，定义为重载函数，使用起来和原来一样方便。
  core::uint32 GetAttribute(RolePublicFields::type type);
  core::uint32 GetAttribute(RolePrivateFields::type type);
  void SetAttribute(RolePublicFields::type type, core::uint32 value);
  void SetAttribute(RolePrivateFields::type type, core::uint32 value);

 private:
  core::uint32 public_fields_[RolePublicFields::PUBLIC_END];
  core::uint32 private_fields_[RolePrivateFields::PRIVATE_END];
};

{% endhighlight %}

使用起来其实可加清晰，可加方便。

{% highlight c++ %}

Skill::Attack(core::uint64 src_id, core::uint64 dest_id,
      core::uint32 skill_id) {
  Role *src_role = RoleManager::GetInstance().GetRole(src_id);
  Role *dest_role = RoleManager::GetInstance().GetRole(src_id);
  if(src_role && dest_role) {
    core::uint32 hurt_value = this->GetBasicHurt(skill_id);
    dest_role->SetAttribute(RolePublicFields::HP,
        dest_role->GetAttribute(RolePublicFields::HP) - hurt_value);
  }
}

{% endhighlight %}

