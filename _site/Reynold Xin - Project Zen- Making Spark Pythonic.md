Speakers:
Malcolm Gladwell
Mae Jemison

# Reynold Xin - Project Zen: Making Spark Pythonic

In 2020,  

- Scala and others 12% JVM based languages
- Python 47%
- SQL 41%

## Pythonic UDFs

type hinting in python 3.8 way

## Error handling

- p.2 -> ZeroDivisionError (actual error happening)
- p.2 Giant JVM stack trace (executor)
- p.3 Giant JVM stack trace (driver)
- ...
- multiple JVM stack trace -> internal to spark

Current progress: Removing JVM stack trace and leave the python ones

## Pythonoic Autocompletion

- context-relevant autocompleting (showing the options)

## Static error detection in IDEs

- made possible in type-hinting
- can scale python development to much larger team

## Docs

- Pythonic Docs
- Adopt NumPy style docs:
  - List all classes, each with one sentence summary
  - Methods has one summary sentence each
  - One page for one method

# Malcolm Gladwell - Why Data Should Drive the Next Pandemic Response

The role of data in solving tough problems

- covid is not handled well in many countries
- because politics first, data second :(
- What we put data first?
- 









