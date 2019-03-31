lager_daily_rotation_file_backend
=====

    It provides an user-defined handler to rotate the log files everyday base on the lager project.

Examplle
-----

    1. configurate in rebar.config file:
    {parse_transform, lager_transform}

    2. configurate in sys.config file:
	[
		{lager, [
				{colored, true},
				{crash_log, "log/crash.log"},
				{crash_log_msg_size, 65536},
				{crash_log_size, 10485760},
				{crash_log_date, "$D0"},
				{crash_log_count, 5},
				{handlers, [
				{lager_console_backend, [
					{level, debug},
					{formatter_config, [time, color, " [", severity, "] ", module, ":", function, ":", line, " ", message, "\e[0m\r\n"]}
				]},
				{lager_file_backend, [
					{level, debug},
					{formatter_config, [date, " ", time, " [", severity, "] ", module, ":", function, ":", line, " ", message, "\n"]},
					{file, "log/console.log"},
					{size, 10485760},
					{date, "$D0"},
					{count, 20}
				]},
				{lager_file_backend, [
					{level, error},
					{formatter_config, [date, " ", time, " [", severity, "] ", module, ":", function, ":", line, " ", message, "\n"]},
					{file, "log/error.log"},
					{size, 10485760},
					{date, "$D0"}
				]},
				{lager_daily_rotation_file_backend, [
					{level, debug},
					{formatter_config, [date, " ", time, " [", severity, "] ", module, ":", function, ":", line, " ", message, "\n"]},
					{path, "log/"}
				]}
			]}
		]}
	].

    3. add lager_daily_rotation_file_backend to xxx.app.src file:
    {applications, [
        kernel,
        stdlib,
        sasl,
        ssl,
        crypto,
        parse_trans,
        lager_daily_rotation_file_backend
    ]}

    4. include lib_hrl file:
    -include_lib("lager_daily_rotation_file_backend/include/log.hrl").
