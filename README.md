```bash
LOG_error=91 LOG_success=92 LOG_warning=93 LOG_info=96 LOG_fatal=31

logger() {
  local type="${1}"
  shift
  local message="$*"
  local color_var="LOG_${type}"
  local color=${!color_var:-${LOG_info}}

  if [[ ! "${color_var}" =~ ^LOG_(error|success|warning|info|fatal)$ || -z "${message}" ]]; then
    echo -e "$(date +"%Y-%m-%d %H:%M:%S,%3N") FATAL\tUsage: logger {error|success|warning|info|fatal} \"Message\""
    return 1
  fi

  if ${CONF_COLORS:-false}; then
    echo -e "$(date +"%Y-%m-%d %H:%M:%S,%3N") \e[${color}m$(echo "${type}" | tr '[:lower:]' '[:upper:]')\e[0m\t${message}"
  else
    echo -e "$(date +"%Y-%m-%d %H:%M:%S,%3N") $(echo "${type}" | tr '[:lower:]' '[:upper:]')\t${message}"
  fi
}

# Tests
logger error "Example error message"     # OK
logger warning "Example warning message" # OK
logger lorem "Incorrect logger"          # FATAL
logger error                             # FATAL
logger error lorem ipsum                 # OK
logger info "Example info message"       # OK
logger success "Example success message" # OK
```
