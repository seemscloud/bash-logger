```bash
#!/bin/bash

LOG_COLORS_error=91 LOG_COLORS_success=92 LOG_COLORS_warning=93 LOG_COLORS_info=96 LOG_COLORS_fatal=31
CONF_COLORS="true"

logger() {
  local type="${1}" message="${2}"
  local color_var="LOG_COLORS_${type}"
  color=${!color_var:-${LOG_COLORS_info}}

  [[ -z "${message}" ]] && {
    echo -e "$(date +"%Y-%m-%d %H:%M:%S,%3N") FATAL\tUsage: logger {error|success|warning|info|fatal} \"Message\""
    return 1
  }

  if [[ "${CONF_COLORS}" == "true" ]]; then
    echo -e "$(date +"%Y-%m-%d %H:%M:%S,%3N") \e[${color}m${type^^}\e[0m\t${message}"
  else
    echo -e "$(date +"%Y-%m-%d %H:%M:%S,%3N") ${type^^}\t${message}"
  fi
}

logger error "Example error message"
logger warning "Example warning message"
logger lorem "Incorrect logger"
logger error
logger error lorem ipsum
logger info "Example info message"
logger success "Example success message"
```
