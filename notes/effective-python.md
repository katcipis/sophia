# Best tips

* Do not use get/set, just plain attributes and the @property decorator
* Create lib specific exceptions to make clear programming errors on the lib from the user using your API wrongly
* Dont silence errors, it is better to allow the exception to bubble up, or handle it properly
* Always specify which exception to catch, dont use just except (it will catch stuff like SystemExit and KeyboardInterrupt)
* Default parameters should not be initialized with mutable values (they are initialized only once)
* Dont do expensive work on import (module body), this avoids a lot of surprises and makes testing easier
