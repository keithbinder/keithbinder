---
layout: post
title:  "Dinner Club for Omaha Code School"
date:   2015-06-17 09:11:10
categories: technical
---
# Dinner Club

The program DinnerClub tracks the outings of it's members and how much they have spent. It also comes with a nifty check splitter that divides the check and tells each person what they owe, with tip of course.

DinnerClub class gets instantiated with two attributes "@members" a Hash with keys of member names and values of the amounts they've spent, and "@history" an Array that stores the information about outings.

```ruby
def initialize(member_names)
    @members = {}
    @history = []
    
    member_names.each do |name|
      @members[name] = 0
    end
  end
```

## Go Out Method

Here we have the meat of the program. The method "go_out" does a lot of the work. Meal Cost stores and Integer that is the base amount of the check. Tip Percent is another Integer that is how much tip will be added. Attendees is an Array of the members the went on the outing. Place Visited is a String of locations that the members went to. One Payer Name is a String indentifying one member if they pick up the whole tab. Bill Split is a String tracking who pays how much when the bill is divided evenly.

Then I instantiate the Check Splitter class, which is stored in its own file.

```ruby
def go_out(meal_cost, tip_percent, attendees, place_visited, one_payer_name, bill_split)
    cs = CheckSplitter.new(meal_cost, tip_percent, attendees.length)
    outing_members = {}
    attendees.each do |attendee|
      outing_members[attendee] = 0
    end
    
    amount_per_person = cs.even_split.round(2)
    single_payer_amount = cs.meal_plus_tip.round(2)
    
    case bill_split
      when "n"
        @members[one_payer_name] += single_payer_amount
        outing_members[one_payer_name] += single_payer_amount
      when "y"
        attendees.each do |a|
          @members[a] = @members[a] + amount_per_person
          @members[a].round(2)
          outing_members[a] = amount_per_person
        end
    end
    
    outing = Outing.new(place_visited, outing_members)
    history(outing)
  end
```