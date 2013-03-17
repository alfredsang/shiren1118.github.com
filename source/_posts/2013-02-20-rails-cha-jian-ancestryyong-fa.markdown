---
layout: post
title: "rails 插件 ancestry用法"
date: 2013-02-20 17:31
comments: true
categories: 
---


简单总结一下用法 是对1.3版本做的



# 第一步增加gem

```ruby
gem 'ancestry'
```

# 第二步对模型增加ancestry:string字段 
```
bundle
rails g migration add_ancestry_to_messages ancestry:string
rake db:migrate
```

修改migration.rb
```ruby
class AddAncestryToMessages < ActiveRecord::Migration
  def self.up
    add_column :messages, :ancestry, :string
    add_index :messages, :ancestry
  end

  def self.down
    remove_index :messages, :ancestry
    remove_column :messages, :ancestry
  end
end
```

# 第三步修改model
```ruby
class Category < ActiveRecord::Base
  attr_accessible :desc, :name ,:parent
  has_ancestry
end
```
注意：增加:parent和has_ancestry
这步是别的教程里没有强调的

# 第四步 测试

```ruby
require 'test_helper'

class CategoryTest < ActiveSupport::TestCase
  # test "the truth" do
  #   assert true
  # end
  
  test "dump category" do
    @b = Category.create! :name => 'bbbbb', :parent => Category.create!(:name => 'aaaaaaaaa')
    @d = Category.create! :name => 'dddddddd', :parent => @b
    @e = Category.create! :name => 'eeeeeee', :parent => @d
    @c = Category.find_by_name 'bbbbb'
    
    # ap @d
    # ap @d.parent
    # ap @d.root

    ap @d.subtree_ids
    # ap @c.parent
    
    
    # parent           Returns the parent of the record, nil for a root node
    # parent_id        Returns the id of the parent of the record, nil for a root node
    # root             Returns the root of the tree the record is in, self for a root node
    # root_id          Returns the id of the root of the tree the record is in
    # is_root?         Returns true if the record is a root node, false otherwise
    # ancestor_ids     Returns a list of ancestor ids, starting with the root id and ending with the parent id
    # ancestors        Scopes the model on ancestors of the record
    # path_ids         Returns a list the path ids, starting with the root id and ending with the node's own id
    # path             Scopes model on path records of the record
    # children         Scopes the model on children of the record
    # child_ids        Returns a list of child ids
    # has_children?    Returns true if the record has any children, false otherwise
    # is_childless?    Returns true is the record has no childen, false otherwise
    # siblings         Scopes the model on siblings of the record, the record itself is included
    # sibling_ids      Returns a list of sibling ids
    # has_siblings?    Returns true if the record's parent has more than one child
    # is_only_child?   Returns true if the record is the only child of its parent
    # descendants      Scopes the model on direct and indirect children of the record
    # descendant_ids   Returns a list of a descendant ids
    # subtree          Scopes the model on descendants and itself
    # subtree_ids      Returns a list of all ids in the record's subtree
    # depth            Return the depth of the node, root nodes are at depth 0
  end
end
```
说明:ap是awesome_print 

在rails console中执行
> rake test


需要什么方法尽可在官方的文档里找，整体看还比较完善

# 参考地址
- https://github.com/stefankroes/ancestry
- http://railscasts.com/episodes/262-trees-with-ancestry



