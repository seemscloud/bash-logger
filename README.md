```bash
#!/bin/bash

declare -A LOG_COLORS=(
  [error]=91 [success]=92 [warning]=93 [info]=96 [fatal]=31
)

CONF_COLORS="true"
{
  function logger_format() {
    local DATE
    DATE=$(date +"%Y-%m-%d %H:%M:%S,%3N")
    [[ "${CONF_COLORS}" == "true" ]] && echo -e "${DATE} \e[${3}m${1}\e[0m\t${2}" || echo -e "${DATE} ${1}\t${2}"
  }
}

function logger() {
  local type="${1}" message="${2}"
  [[ -z "${message}" || -z "${LOG_COLORS[${type}]}" ]] && {
    logger_format "FATAL" "Usage: logger {error|success|warning|info|fatal} 'Message'" 31
    return 1
  }
  logger_format "${type^^}" "${message}" "${LOG_COLORS[${type}]}"
}

logger error "Example error message"
logger warning "Example warning message"

logger lorem "Incorrect logger"
logger error
logger error lorem ipsum

logger info "Example info message"
logger success "Example success message"
```
