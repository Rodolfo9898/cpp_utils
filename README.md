*This project has been created as part of the 42 curriculum by rperez-t.*

# cpp_utils

## Description

The extra material of the C++ modules (CPP00 to CPP09), kept in one place: the files a
subject attaches on the intranet (tarballs, given headers, test mains, reference logs),
and the small tools I use to evaluate an exercise. None of it is part of what a student
turns in.

It is meant to be added to a module repository as a submodule, under `eval_helper/`. The
evaluation then has everything it needs inside the project, without downloading anything
during the review.

### Layout

One folder per module, then per exercise, with the files kept as the subject gives them:

```
cpp_utils/
├── cpp00/
│   └── ex02/           # the attached tarball and what it contains
│       ├── Account.hpp
│       ├── tests.cpp
│       └── 19920104_091532.log
├── cpp01/
│   └── ...
└── README.md
```

For example, CPP00 ex02 ("The Job Of Your Dreams") gives `Account.hpp`, `tests.cpp` and
the log of a run; the student only writes `Account.cpp`. Keeping the originals here lets
an evaluator check that the given files were not modified:

```sh
diff eval_helper/cpp_utils/cpp00/ex02/Account.hpp ex02/Account.hpp
diff eval_helper/cpp_utils/cpp00/ex02/tests.cpp   ex02/tests.cpp
```

And compare the output with the reference log, timestamps removed (the subject says they
will differ, and that the order of the destructors may too):

```sh
cd ex02 && make
./<program> | cut -d' ' -f2- > /tmp/mine.txt
cut -d' ' -f2- ../eval_helper/cpp_utils/cpp00/ex02/19920104_091532.log > /tmp/ref.txt
diff /tmp/ref.txt /tmp/mine.txt
```

## Instructions

### Adding it to a module

From the root of the module repository (for example `cpp00`):

```sh
git submodule add https://github.com/Rodolfo9898/cpp_utils.git eval_helper/cpp_utils
git commit -m "Add cpp_utils as eval helper"
```

### Cloning a module that already has it

```sh
git clone --recurse-submodules <module-url>
# or, in an existing clone
git submodule update --init --recursive
```

### Updating it

After adding new files here, bring a module up to the latest commit:

```sh
git submodule update --remote eval_helper/cpp_utils
git add eval_helper/cpp_utils
git commit -m "Update cpp_utils"
```

### Adding files

1. Download the attachment of the subject from the intranet.
2. Extract it into `cppXX/exYY/`, keeping the original file names. Keep the tarball
   itself next to them if you want to be able to check it later.
3. Commit with the module and exercise in the message, for example
   `cpp00/ex02: add the attached files`.

Do not put solutions here: only what the subject gives, and tools that check a project.

### Troubleshooting

| Symptom | Cause and fix |
|---|---|
| Empty `eval_helper/cpp_utils` folder | The submodule was not fetched: `git submodule update --init` |
| A module still shows old files | The submodule is pinned to a commit: `git submodule update --remote` |
| `diff` on the log shows every line | Timestamps were not stripped, or the line endings differ (`dos2unix`) |

## Resources

- The subjects of the C++ modules, CPP00 to CPP09, on the intranet
- Git submodules - https://git-scm.com/book/en/v2/Git-Tools-Submodules
- `man diff`, `man cut`, `man tar`
